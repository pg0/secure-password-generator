# secure random password generator

secure javascript only - random password generator

using crypto.getRandomNumber() instead of Math.random()


try it yourself: 
https://htmlpreview.github.io/?https://github.com/pg0/secure-password-generator/blob/26c37f8e17c5ebd4b793e4d060f8de58489e37b5/pwgen.html

``` html
<div class="container">
Passwords: <input type="number" id="pwamount" value="4" /><br />
<br />
PW Length: <input type="number" id="pwlength" value="14" /><br />
Ambiguous: <input type="checkbox" id="pwambigious" />`Il1O0`<br />
Lowercase: <input type="checkbox" id="pwlowercase" checked="checked" />`[a-z]`<br />
Uppercase: <input type="checkbox" id="pwuppercase" checked="checked" />`[A-Z]`<br />
Numbers: <input type="checkbox" id="pwnumbers" checked="checked" />`[0-9]`<br />
Easy Symbols: <input type="checkbox" id="pweasysymbols" checked="checked" /> `!@#$%*+-=?`<br />
Other Symbols: <input type="checkbox" id="pwothersymbols" /> `^&_:|~/.,;`<br />
Individual: <input type="checkbox" id="pwindividual" /><input type="text" id="pwindiv" value=".,!$§#?" /><br />
<br />
<button id="btnGenerate" class="generate" onclick="genPwd()">generate</button><br />
<br />
<div id="pwres">&nbsp;</div>
<span class="smalltext">Click on Password to Copy to Clipboard</span>
</div>
```

``` javascript
// generate multiple passwords
function genPwd() {
  let pwamount = document.querySelector('#pwamount').value;
  let pwlength = document.querySelector('#pwlength').value;
  let includeLowercase = document.querySelector('#pwlowercase').checked;
  let includeUppercase = document.querySelector('#pwuppercase').checked;
  let includeNumbers = document.querySelector('#pwnumbers').checked;
  let includeAmbiguous = document.querySelector('#pwambigious').checked;
  let includeEasySymbols = document.querySelector('#pweasysymbols').checked;
  let includeOtherSymbols = document.querySelector('#pwothersymbols').checked;
  let includeIndividual = document.querySelector('#pwindividual').checked;

  document.querySelector('#pwres').innerHTML = '';
  for (i=0; i < pwamount; i++) {
    let pwresult = generateSecurePassword(pwlength,includeLowercase,includeUppercase,includeNumbers,includeAmbiguous,includeEasySymbols,includeOtherSymbols,includeIndividual);
    
    let nladdon = '';
    if (i > 0) nladdon = '<br/>';
    document.querySelector('#pwres').innerHTML += nladdon + '<span class="clickme" onclick="copyToClipboard(this.innerText)">'+ filterSpecChars(pwresult) +'</span>';
  }
}

// copy value to clipboard
function copyToClipboard(value){
  const textarea = document.createElement('textarea');
  textarea.value = value;
  document.body.appendChild(textarea);
  textarea.select();
  document.execCommand('copy');
  textarea.remove();
}

// Secure Random Password Generator
function generateSecurePassword(length=10, includeLowercase=true, includeUppercase=false, includeNumbers=false, includeAmbiguous=false, includeEasySymbols=false, includeOtherSymbols=false, includeIndividual=false, secure=true) {
  let charset = ""
  if (includeLowercase) charset += "abcdefghjkmnpqrstuvwxyz";
  if (includeLowercase && includeAmbiguous) charset += "iol";
  if (includeUppercase) charset += "ABCDEFGHJKMNPQRSTUVWXYZ";
  if (includeUppercase && includeAmbiguous) charset += "IOL";
  if (includeNumbers) charset += "123456789";
  if (includeNumbers && includeAmbiguous) charset += "0";
  if (includeEasySymbols) charset += "!@#$%*+-=?";
  if (includeOtherSymbols) charset += "^&_:|~/.,;";
  if (includeIndividual) charset += document.querySelector('#pwindiv').value;

  if(window.crypto && window.crypto.getRandomValues)
  {
      var result = "";
      let values = new Uint32Array(length);
      window.crypto.getRandomValues(values);
      for(let i=0; i<length; i++)
      {
          result += charset[values[i] % charset.length];
      }
      
      return result;
  }
  else throw new Error("Your browser can't generate secure random numbers");
};

// sometimes makes problems showing all chars
function filterSpecChars(str) {
return str.replace(/>/g, "&gt;").replace(/</g, "&lt;");
}
```
