# SHABAN AI ACADEMY — Automatic UPI Checkout

## इसमें क्या है
- `public/payment.html`: आपकी मौजूदा payment page का वही dark design, ₹21 price और branding; manual screenshot/UTR form हटाकर BUY NOW checkout।
- `server.js`: Razorpay order creation, server-side signature verification, Razorpay से captured payment fetch, SQLite enrollment record और signed course access token को HttpOnly cookie में सेट करना।
- `public/course.html`: protected course landing page का starter।

## सबसे जरूरी सीमाएँ
1. यह live payment system तभी चलेगा जब Razorpay merchant account approve हो और API keys मिलें।
2. Razorpay Dashboard में UPI methods/UPI Intent उपलब्धता account और current policy पर निर्भर करती है। PhonePe, Google Pay, Paytm सभी फोन पर guaranteed नहीं हैं; Checkout user के device पर supported apps दिखाता है।
3. Netlify static hosting अकेले secrets, order creation या payment verification सुरक्षित रूप से नहीं चला सकती। इस Node backend को Render/Railway/Fly.io या अपने Node host पर HTTPS के साथ deploy करें।
4. Course की असली videos/files को public folder या public Google Drive link में रखने से वे payment के बिना भी share हो सकती हैं। असली protection के लिए private video hosting/signed URLs या authenticated streaming लगाएँ।
5. Test mode में ₹21 का वास्तविक payment नहीं होगा। पहले test keys से test करें, फिर live keys डालें।

## Setup
1. Razorpay merchant account बनाकर KYC/settlement details पूरी करें। यदि account holder नाबालिग है, तो parent/guardian के नाम और उनकी अनुमति/नियमों के अनुसार account चलाएँ।
2. इस folder में terminal खोलें और `npm install` चलाएँ।
3. `.env.example` की copy `.env` नाम से बनाएँ। Razorpay Dashboard की Test API Key ID और Secret भरें। `JWT_SECRET` में कम-से-कम 32 random characters रखें।
4. `public/payment.html` को `public` folder में रखें (यह package में पहले से है)।
5. `npm start` चलाएँ; local test के लिए `http://localhost:3000/payment.html` खोलें।
6. Test payment सफल होने के बाद Razorpay Dashboard से live activation करें और `.env` में live keys डालें।
7. Razorpay Dashboard में webhook URL `https://YOUR-BACKEND-DOMAIN/api/razorpay-webhook` और `payment.captured` event configure करें; webhook secret `.env` में रखें।
8. Backend HTTPS पर deploy करें। यदि frontend अलग Netlify domain पर है, तो CORS, credentials, secure token handoff और exact allowed origin configure करना होगा; इस starter में same-origin hosting सबसे सरल है।

## Course page
`public/course.html` में अपने lessons जोड़ें। Payment के बाद signed access token HttpOnly cookie में सेट होता है। Browser/device बदलने पर customer को दोबारा access देने के लिए login + database-based enrollment flow जोड़ना बेहतर है। Production में access token URL query में न डालें।
