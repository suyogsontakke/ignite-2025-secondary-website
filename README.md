<div align="center">
  <img src="https://i.postimg.cc/HkXcxcgX/1758263167091.png" alt="IGNITE 2025 Logo" width="150" />
  <h1>IGNITE 2025 - Event Ticket Booking Site (UPI Version)</h1>
  <p>
    An alternate version of the IGNITE 2025 booking site using a manual UPI QR code payment flow and WhatsApp for ticket confirmation.
  </p>
  
  <p>
    <a href="https://github.com/suyogsontakke/ignite-2025-secondary-website/releases"><img src="https://img.shields.io/github/v/release/suyogsontakke/ignite-2025-secondary-website?label=Latest%20release&style=flat-square" alt="Latest release"/></a>
    <a href="https://github.com/suyogsontakke/ignite-2025-secondary-website/stargazers"><img src="https://img.shields.io/github/stars/suyogsontakke/ignite-2025-secondary-website?style=flat-square" alt="Stars"/></a>
    <a href="https://github.com/suyogsontakke/ignite-2025-secondary-website/forks"><img src="https://img.shields.io/github/forks/suyogsontakke/ignite-2025-secondary-website?style=flat-square" alt="Fork"/></a>
    <a href="https://github.com/suyogsontakke/ignite-2025-secondary-website/blob/main/LICENSE"><img src="https://img.shields.io/github/license/suyogsontakke/ignite-2025-secondary-website?style=flat-square&color=blue" alt="License"/></a>
    <img src="https://img.shields.io/github/languages/top/suyogsontakke/ignite-2025-secondary-website?style=flat-square&logo=html5&logoColor=white&color=E34F26" alt="Top Language: HTML"/>
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square" alt="PRs welcome"/>
  </p>
  
  <strong>Live Demo: <a href="#">\[YOUR_LIVE_DEMO_LINK_HERE\]</a></strong>
</div>

![IGNITE 2025 UPI Version Screenshot](https://i.postimg.cc/1tRnYkJK/image.png)

---

## 📖 Table of Contents

<details><summary>Table of Contents</summary>

* [🚀 About This Project](#-about-this-project)
* [✨ Key Features](#-key-features)
* [🛠️ Technologies Used](#️-technologies-used)
* [🔗 System Architecture](#-system-architecture)
* [🧰 Getting Started](#-getting-started)
  * [📋 Prerequisites](#-prerequisites)
  * [⚙️ Installation & Setup](#️-installation--setup)
  * [▶️ Run Locally](#️-run-locally)
* [🔒 Configuration](#-configuration)
* [🚀 Deployment](#-deployment)
* [🤝 Contributing](#-contributing)
* [📄 License](#-license)
* [💎 Acknowledgements](#-acknowledgements)

</details>

---

## 🚀 About This Project

This repository contains an **alternate version** of the public-facing **Ticket Booking Website** for the "IGNITE 2025" event. This version replaces the automated Razorpay payment and ticket generation flow with a **manual UPI payment process** using QR codes and relies on **WhatsApp** for payment confirmation and ticket delivery by administrators.

It still provides event details, promotional content, and AI-driven support ("Dolly" chatbot). Built with **Vanilla JavaScript**, **Tailwind CSS**, and HTML5, it offers a responsive experience while simplifying the payment integration.

When a user registers, their details are saved to the `pending_registrations` collection in Firebase Firestore with a `pending_payment` status. They are then shown a UPI QR code and instructed to send a payment screenshot via WhatsApp to a designated number for manual verification and ticket issuance by the event administrators.

**Note:** This version requires more manual intervention from the event organizers compared to the primary version integrating Razorpay.

---

## ✨ Key Features

* **Responsive Design:** Fully responsive layout using **Tailwind CSS**.
* **Event Information:** Displays event details, venue, menu, team, partners, etc.
* **Manual UPI Payment Flow:**
    * Collects attendee details (Name, Email, Mobile).
    * Saves initial registration data to Firebase Firestore (`pending_registrations` with `status: 'pending_payment'`).
    * Displays a **static UPI QR code** for the required payment amount.
    * Provides buttons/links to easily switch between different QR codes/payment amounts if needed (e.g., student vs. outsider).
    * Instructs the user to **manually scan the QR code** with their UPI app.
    * Provides a **WhatsApp link** pre-filled with a message for the user to send their payment screenshot for verification.
* **AI Chatbot Assistant ("Dolly"):** Integrated chatbot powered by the **Google Gemini API** to answer event questions.
* **Serverless Backend (Partial):** Uses **Firebase Firestore** to log initial registration attempts before payment confirmation.
* **Firebase Anonymous Authentication:** Allows users to write their initial registration details to Firestore securely.

*(This version **does not** include automatic payment verification, ticket generation (QR code, PDF/JPG download) on the client-side).*

---

## 🛠️ Technologies Used

<details><summary>This project utilizes the following technologies and services:</summary>

* [**HTML5**](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5): Standard markup language.
* [**Tailwind CSS**](https://tailwindcss.com/): Utility-first CSS framework (via CDN).
* [**JavaScript (ES6+)**](https://developer.mozilla.org/en-US/docs/Web/JavaScript): For form handling, UI updates, API calls. Uses Modules.
* [**Firebase Firestore**](https://firebase.google.com/products/firestore): Used to store initial registration details in `pending_registrations`.
* [**Firebase Authentication**](https://firebase.google.com/products/auth): Used for **Anonymous Authentication** for Firestore writes.
* [**Google Gemini API**](https://ai.google.dev/): Powers the "Dolly" AI chatbot.
* **Static Images:** Uses pre-generated UPI QR code images hosted externally (e.g., via `postimg.cc`).
* **WhatsApp Click-to-Chat:** Utilizes WhatsApp's URL scheme to open a chat for sending payment proof.

</details>

[![Technologies Used](https://skillicons.dev/icons?i=html,tailwind,js,firebase,googlecloud)](https://skillicons.dev)
*(Note: 'googlecloud' icon represents the Gemini API)*

---

## 🔗 System Architecture

This UPI version fits into the event platform as follows:

1.  **Booking Site (This Repo):** Attendee registers -> Data saved to Firestore (`pending_registrations`, status `pending_payment`) -> User shown UPI QR -> User pays manually -> User sends screenshot via WhatsApp.
2.  **Admin (Manual Process):** Receives WhatsApp message -> Verifies payment screenshot -> Finds corresponding entry in Firestore `pending_registrations` -> Uses the **Admin Dashboard** (Separate App) to manually approve the registration (moving data to `approved_registrations`) -> Manually generates and sends the ticket (e.g., via WhatsApp or Email).
3.  **Ticket Verifier (Separate App):** Scans ticket QR code -> Verifies against `approved_registrations` -> Updates check-in status.

**Security:** Firestore rules ensure this site can only create entries in `pending_registrations`. Admins require separate, secure login for approval and ticket generation.

---

## 🧰 Getting Started

To get this UPI version booking website running locally.

### 📋 Prerequisites

* A **Firebase Project** with Firestore and Anonymous Authentication enabled.
* **UPI QR Code Images:** You need images of your UPI QR code(s) hosted online (e.g., using [postimages.org](https://postimages.org/)).
* **WhatsApp Number:** The phone number where users should send payment screenshots.
* A **Google AI API Key** for the Gemini API (Chatbot functionality).
* [Node.js](https://nodejs.org/en/) (Optional: for using a local development server)
* [Git](https://git-scm.com/downloads)

### ⚙️ Installation & Setup

1.  **Clone the Repo**
   
    ```bash
    git clone https://github.com/suyogsontakke/ignite-2025-secondary-website.git
    cd ignite-2025-secondary-website
    ```

2.  **Configure Firestore Rules**
    * Go to your **Firebase Console** -> **Firestore Database** -> **Rules**.
    * Paste the contents of the `FIRESTORE.rules` file provided in this repository. Ensure it allows anonymous users to create documents in `pending_registrations` with the specified fields (`name`, `email`, `mobile`, `status`, `registeredAt`).
    * **Publish** the rules.

3.  **Configure `index.html`**
    * Open `index.html` in a code editor.
    * Find the `<script type="module">` section.
    * **Firebase Config:** Locate the `firebaseConfig` object and replace placeholders with your actual Firebase project keys.
    * **Payment Details:** Find the `studentPaymentDetails` (and potentially add others like `outsiderPaymentDetails`). Update the following:
        * `price`: The amount string (e.g., `"750"`).
        * `qrCodeUrl`: The direct URL to your hosted UPI QR code image.
        * `whatsappNumber`: The phone number (including country code, no + or spaces) for receiving screenshots.
        * `entryType`: A descriptive name (e.g., `"Student Entry"`).
        ```javascript
        const studentPaymentDetails = {
            price: "750",
            qrCodeUrl: "YOUR_STUDENT_QR_CODE_IMAGE_URL", // <-- Replace
            whatsappNumber: "YOUR_WHATSAPP_NUMBER",     // <-- Replace
            entryType: "Student Entry"
        };
        // Add more details objects if needed
        ```
    * **Gemini API Key:** Find the `getDollyResponse` function. Replace `"YOUR_GEMINI_API_KEY"` with your actual Google AI API key.
        ```javascript
        async function getDollyResponse(userMessage) {
            const apiKey = "YOUR_GEMINI_API_KEY"; // <-- Replace this
            // ... rest of the function
        }
        ```
    * **(Optional) Chatbot Prompt:** Modify the `systemPrompt` variable if needed.
    * **(Optional) QR Toggle Buttons:** Adjust the HTML and `updateUpiDetails` logic if you have multiple ticket types/QR codes.

### ▶️ Run Locally

* **Simple Method:** Open the `index.html` file directly in your web browser.
* **Recommended Method (using a local server):**
    ```bash
    npx live-server
    # OR if installed globally:
    # live-server
    ```
    Navigate to the local URL provided (e.g., `http://127.0.0.1:8080`).

---

## 🔒 Configuration

Key configuration points are directly within `index.html`:

* **`firebaseConfig` Object:** Firebase project credentials.
* **Payment Details Objects (`studentPaymentDetails`, etc.):** Contain Price, QR Image URL, WhatsApp Number, and Entry Type.
* **`Gemini API Key`:** Used for the AI Chatbot. **Treat this as sensitive.** Consider backend proxy for production.

---

## 🚀 Deployment

Deploy this static website using any modern hosting provider like Vercel, Netlify, Firebase Hosting, or GitHub Pages.

```bash
# Example Firebase Deployment
firebase deploy --only hosting
```

## 🤝 Contributing
Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

1. Fork the Project

2. Create your Feature Branch (git checkout -b feature/AmazingFeature)

3. Commit your Changes (git commit -m 'Add some AmazingFeature')

4. Push to the Branch (git push origin feature/AmazingFeature)

5. Open a Pull Request

## 📄 License

Distributed under the MIT License. See LICENSE file for more information.

## 💎 Acknowledgements

● Firebase

● Tailwind CSS

● Google AI (Gemini)

● Font Families: Inter, Bebas Neue
