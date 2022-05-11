## Step 1) Set Up Project Folder
To get started, you can use any web development IDE. 
1. Create a New Project. 
2. Create the following new files: 
  * `index.html`
  * `main.css`
  * `script.js`
3. Add general structure to `index.html` with a link to your CSS file:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Login</title>
  <link rel = "stylesheet" href = "main.css">
</head>
<body>
</body>
</html>
```

## Step 2) Set Up [Firebase](https://firebase.google.com/) Project. 
If you don't already have an account, create one before starting. 
1. Click `Go to Console` &rarr; `Add Project` &rarr; Name your project.
2. Click `Continue` &rarr; `Continue`.
3. Choose `Default Account for Firebase`.
4. Click `Create Project`. 
5. When your project is ready, click `Complete`. 

## Step 3) Connect Firebase to Your Project
1. In the `Build` menu, select `Authentication`. 
2. Click `Get Started`.
3. In the `Sign-In Method` tab, select `Email/Password`. 
4. Toggle the switch to `Enable`. 
5. Click `Save`. 
6. In `Project Settings`, scroll down to `Your apps`. 
7. Choose `Web`. 
8. In **Step 1, Register app**:
    * Name your app &rarr; Click `Register app`. 
9. In **Step 2, Add Firebase SDK**:
    * Select `Use a <script> tag`.
    * Copy the code snippet. 
    * For now, paste it in `script.js`. The errors will be fixed when you revisit the JS file in **Step 5**. 
    * Click `Continue to console`.
    
## Step 4) HTML 
The structure for this page is pretty simple. We'll have a container with 2 divs inside it. The site will switch the visibility of the divs depending on the task that the user is trying to complete. 

1.**Within the** `body`**, add a container for the log in elements:** 
```html
<div class = "login-container">
</div>
```
2. **Within** `.login-container`**, you'll need 2 additional divs:** `#main` **and** `#create-acct`**:** 
```html
<div id= "main">
</div>

<div id = "create-acct">
</div>
```
3. `#main` **will be used to store the sign in elements:**
```html
<h1>Sign In</h1>
<input id = "email" type = "text" placeholder = "Email"></input>
<input id = "password" type = "password" placeholder = "Password"></input>
<button id = "submit">Submit</button>
<p><span>or</span></p>
<button id = "sign-up">Sign Up</button>
```
4. `#create-acct` **will be used to store the create account elements:** 
```html
<h1>Create an Account</h1>
<input id = "email-signup" type = "text" placeholder = "Email *"></input>
<input id = "confirm-email-signup" type = "email" placeholder = "Confirm Email *"></input>
<input id = "password-signup" type = "password" placeholder = "Password *"></input>
<input id = "confirm-password-signup" type = "password" placeholder = "Confirm Password *"></input>
<button id = "create-acct-btn">Create Account</button>
<button id = "return-btn">Return to Login</button>
```

5. **At the bottom of the body, link `script.js`.**
```html
 <script type = "module" src = "script.js"></script>
```

 **2 things to note here:**
 1. Adding "module" as the type is an important step to make sure that we can import methods from firebase later. 
 2. If you attempt to run your project locally with the script linked in your HTML, you will get an error. More detail on this in **Step 5**.
 
## Step 5) CSS

**Remove the default margins, padding, and box-sizing from all of the elements on the page.**
``` css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: sans-serif;
}
```

**Choose a background color for the body of the page.**
``` css
body {
  background-color: lightblue;
}
```

**Center-align the main heading ("Log In") and add a bottom margin.**
``` css
h1 {
  margin-bottom: 8%;
  text-align: center;
}
```

**Style the paragraph element ("or") to be center with horizontal lines on each side.**
``` css
p {
  margin-top: 5%;
  margin-bottom: 5%;
  width: 100%;
  text-align: center;
  border-bottom: 1px solid #000;
  line-height: 0.1em;
}

p span {
  background:#fff;
  padding:0 10px;
}
```

**Add some breathing room around the input fields.**
``` css
input {
  margin-bottom: 3%;
}

input:last-of-type {
  margin-bottom: 0;
}

input, button {
  padding: 3%;
  width: 100%;
}
```

**Vertically and horizontally center the `login-container`.**
``` css
.login-container {
  background-color: white;
  padding: 7%;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  /* horizontal align */
  width: 40vw;
  margin-left: 30vw;
  /* vertical align */
  height: 70vh;
  margin-top: 15vh;
}
```

**Hide the `create-acct` div on the original loading of the page.**
```css
#create-acct {
  display: none;
}

```

**Style the `submit`, `create-acct-btn` and `return-btn`.**
``` css
button:hover {
  cursor: pointer;
  opacity: 0.8;
  transition: 0.3s;
}

#submit, #create-acct-btn {
  background-color: #0583D2;
  color: white;
  border: none;
  margin-top: 5%;
}

#return-btn {
  background: none;
  color: grey;
  text-decoration: underline;
  border: none;
  margin-top: 3%;
}

#sign-up {
  border: none;
}
```
 
## Step 6) JavaScript
``` java
import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.10/firebase-app.js";
import { getAnalytics } from "https://www.gstatic.com/firebasejs/9.6.10/firebase-analytics.js";
import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/9.6.10/firebase-auth.js";

const firebaseConfig = {
  apiKey: "AIzaSyCybmXAUWgGDCMNQWvcRdaMgE31I1GkF8M",
  authDomain: "log-in-authentication-ac1b6.firebaseapp.com",
  projectId: "log-in-authentication-ac1b6",
  storageBucket: "log-in-authentication-ac1b6.appspot.com",
  messagingSenderId: "735126972855",
  appId: "1:735126972855:web:b26c16bd1de14bf361e032",
  measurementId: "G-3GKSESXV7S"
};

const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
const auth = getAuth(app);

const submitButton = document.getElementById("submit");
const signupButton = document.getElementById("sign-up");
const emailInput = document.getElementById("email");
const passwordInput = document.getElementById("password");
const main = document.getElementById("main");
const createacct = document.getElementById("create-acct")

const signupEmailIn = document.getElementById("email-signup");
const confirmSignupEmailIn = document.getElementById("confirm-email-signup");
const signupPasswordIn = document.getElementById("password-signup");
const confirmSignUpPasswordIn = document.getElementById("confirm-password-signup");
const createacctbtn = document.getElementById("create-acct-btn");

const returnBtn = document.getElementById("return-btn");

var email, password, signupEmail, signupPassword, confirmSignupEmail, confirmSignUpPassword;

createacctbtn.addEventListener("click", function() {
  var isVerified = true;

  signupEmail = signupEmailIn.value;
  confirmSignupEmail = confirmSignupEmailIn.value;
  if(signupEmail != confirmSignupEmail) {
      window.alert("Email fields do not match. Try again.")
      isVerified = false;
  }

  signupPassword = signupPasswordIn.value;
  confirmSignUpPassword = confirmSignUpPasswordIn.value;
  if(signupPassword != confirmSignUpPassword) {
      window.alert("Password fields do not match. Try again.")
      isVerified = false;
  }
  
  if(signupEmail == null || confirmSignupEmail == null || signupPassword == null || confirmSignUpPassword == null) {
    window.alert("Please fill out all required fields.");
    isVerified = false;
  }
  
  if(isVerified) {
    createUserWithEmailAndPassword(auth, signupEmail, signupPassword)
      .then((userCredential) => {
      // Signed in 
      const user = userCredential.user;
      // ...
      window.alert("Success! Account created.");
    })
    .catch((error) => {
      const errorCode = error.code;
      const errorMessage = error.message;
      // ..
      window.alert("Error occurred. Try again.");
    });
  }
});

submitButton.addEventListener("click", function() {
  email = emailInput.value;
  console.log(email);
  password = passwordInput.value;
  console.log(password);

  signInWithEmailAndPassword(auth, email, password)
    .then((userCredential) => {
      // Signed in
      const user = userCredential.user;
      console.log("Success! Welcome back!");
      window.alert("Success! Welcome back!");
      // ...
    })
    .catch((error) => {
      const errorCode = error.code;
      const errorMessage = error.message;
      console.log("Error occurred. Try again.");
      window.alert("Error occurred. Try again.");
    });
});

signupButton.addEventListener("click", function() {
    main.style.display = "none";
    createacct.style.display = "block";
});

returnBtn.addEventListener("click", function() {
    main.style.display = "block";
    createacct.style.display = "none";
});
```
