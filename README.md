# PrimusBank App

## Description
A lightweight banking application built with HTML, Bootstrap, JavaScript, and Firebase. This web app allows users to create accounts, make transactions, and view their transaction history with a clean, responsive interface.

## Features
- User authentication (signup/login)
- Account balance tracking
- Money transfer between users
- Transaction history
- Responsive design for mobile and desktop

## Technologies Used
- HTML5
- Bootstrap 5
- JavaScript (ES6+)
- Firebase
  - Authentication
  - Firestore (database)
  - Hosting

## Project Setup
1. Clone the repository
```bash
git clone https://github.com/Nebenmor/PrimusBank.git
cd PrimusBank
```

2. Firebase Setup
```javascript
// Add this to your firebase-config.js
const firebaseConfig = {
    const firebaseConfig = {
        apiKey: "AIzaSyCO6eht5DllhUaodOFfU_yrhIjMPzrI860",
        authDomain: "firebank-d29c6.firebaseapp.com",
        databaseURL: "https://firebank-d29c6-default-rtdb.firebaseio.com",
        projectId: "firebank-d29c6",
        storageBucket: "firebank-d29c6.firebasestorage.app",
        messagingSenderId: "956045682523",
        appId: "1:956045682523:web:55639153b31e6635df9c7e"
      };
};
```

3. Install Firebase CLI (optional, for deployment)
```bash
npm install -g firebase-tools
firebase login
firebase init
```

## Project Structure
```
simple-banking-app/
│
│
├── js/
│   ├── auth.js
│   ├── transactions.js
│   ├── firebase-config.js
│   └── app.js
│
├── pages/
│   ├── AdminPanel.html 
    ├── CreateNewAccount.html
│   ├── AccountDetails.html
    ├── DepositWithdrawal.html
│   └── Tranasactions.html
│
└── README.md
```

## Firebase Database Structure
```
CustomerList/
    ├── userId/
    │   ├── AccountNbr
    │   ├── AccountTitle
    │   ├── Balance
    │   ├── Cnic
    │   ├── Email
    │   └── Phone
    │

EmployeeList/
    ├── userId/
    │   ├── Email
    │   └── FullName
    │
Transactions/
    ├── transactionId/
        ├── AccontNbr
        ├── AccountTitle
        ├── Amount
        ├── DateTime
        ├── OfficerEmail
        ├── OfficerName
        └── TransactionType
```

## Key Features Implementation

### Authentication
```javascript
// Example of user signup
function signUp(email, password) {
    firebase.auth().createUserWithEmailAndPassword(email, password)
        .then((userCredential) => {
            // Create user profile in Firestore
            return db.collection('users').doc(userCredential.user.uid).set({
                email: email,
                balance: 0,
                accountCreated: new Date()
            });
        })
        .catch((error) => console.error(error));
}
```

### Transactions
```javascript
// Example of money transfer
async function transferMoney(fromId, toId, amount) {
    const batch = db.batch();
    
    const fromRef = db.collection('users').doc(fromId);
    const toRef = db.collection('users').doc(toId);
    
    batch.update(fromRef, { balance: firebase.firestore.FieldValue.increment(-amount) });
    batch.update(toRef, { balance: firebase.firestore.FieldValue.increment(amount) });
    
    await batch.commit();
}
```

## Security Rules
Add these rules to your Firebase Console:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }
    match /transactions/{transactionId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
    }
  }
}
```

## Local Development
1. Open `index.html` in your browser
2. Enable Firebase local emulator (optional):
```bash
firebase emulators:start
```

## Deployment
```bash
# Deploy to Firebase Hosting
firebase deploy
```

## Contributing
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License
MIT License

## Contact
- Developer: [Anthony Nebenmor]
- Email: nebenmor.anthony@gmail.com
- LinkdIn: https://www.linkedin.com/in/anthony-nebenmor/
- GitHub: [Anthony Nebenmor](https://github.com/Nebenmor)

## Screenshots
![Login Page](./Screenshots/LoginPage.png)
![CreateNewAccount Page](./Screenshots/CreateNewAccount.png)
![AccountDetails Page](./Screenshots/AccountDetails.png)
![Deposit Withdrawal Page](./Screenshots/DepositWithdrawal.png)
![Transactions Page](./Screenshots/Transactions.png)