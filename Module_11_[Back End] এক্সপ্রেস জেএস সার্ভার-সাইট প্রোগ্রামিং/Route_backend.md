# 🏬 স্বপ্ননগর মলের গল্প: Express.js-এ Route — MERN Stack Backend-এর পথ-নকশা


> MERN / Full Stack Backend-এর প্রায় পুরো কাজটাই আসলে **Route**-কে ঘিরে: কোন ঠিকানায় কোন কাজ, কে করবে, কার অনুমতি লাগবে, ভুল হলে কী হবে। তাই এই ফাইলে আমরা Route-এর **একদম গোড়া থেকে** শুরু করে বাস্তব একটা প্রজেক্ট (MongoDB + JWT + React) পর্যন্ত যাবো।
> আগের ফাইল (Request Handling ও Multer) না পড়া থাকলেও সমস্যা নেই — যা যা দরকার সব এখানেই আছে। শেষে আছে **প্রশ্ন ও উত্তর** অংশ, নিজেকে যাচাই করার জন্য।


---

## 📚 সূচিপত্র (Table of Contents)

1. [ভূমিকা: স্বপ্ননগর মলের গল্প](#ভূমিকা-স্বপ্ননগর-মলের-গল্প)
2. [১. Route কাকে বলে — তিনটা উপাদান](#১-route-কাকে-বলে--তিনটা-উপাদান)
3. [২. Frontend Route বনাম Backend Route](#২-frontend-route-বনাম-backend-route)
4. [৩. প্রজেক্ট সেটআপ ও Express 5](#৩-প্রজেক্ট-সেটআপ-ও-express-5)
5. [৪. HTTP Method — কাজের ধরন](#৪-http-method--কাজের-ধরন)
6. [৫. Route Path — ঠিকানার ধরন](#৫-route-path--ঠিকানার-ধরন)
7. [৬. Request-এর তথ্য — চার পকেট](#৬-request-এর-তথ্য--চার-পকেট)
8. [৭. Response ও Status Code — উত্তর পাঠানো](#৭-response-ও-status-code--উত্তর-পাঠানো)
9. [৮. Handler, `next()` ও Route-এর ক্রম](#৮-handler-next-ও-route-এর-ক্রম)
10. [৯. `app.use` বনাম `app.get`, `app.all` আর `app.route()`](#৯-appuse-বনাম-appget-appall-আর-approute)
11. [১০. `express.Router()` — তলা ম্যানেজার](#১০-expressrouter--তলা-ম্যানেজার)
12. [১১. Controller — কর্মীর কাজ আলাদা ফাইলে](#১১-controller--কর্মীর-কাজ-আলাদা-ফাইলে)
13. [১২. Middleware দিয়ে Route পাহারা — Auth ও Role](#১২-middleware-দিয়ে-route-পাহারা--auth-ও-role)
14. [১৩. পুরো প্রজেক্ট: BookNest API-র কাঠামো ও Model](#১৩-পুরো-প্রজেক্ট-booknest-api-র-কাঠামো-ও-model)
15. [১৪. Books Route ও Controller](#১৪-books-route-ও-controller)
16. [১৫. Auth Route — সদস্য হওয়া ও লগইন](#১৫-auth-route--সদস্য-হওয়া-ও-লগইন)
17. [১৬. Nested Route — বইয়ের ভেতরে রিভিউ](#১৬-nested-route--বইয়ের-ভেতরে-রিভিউ)
18. [১৭. Error Handling — ৪০৪ আর অভিযোগ ডেস্ক](#১৭-error-handling--৪০৪-আর-অভিযোগ-ডেস্ক)
19. [১৮. `app.js` ও `server.js` — সব জোড়া লাগানো](#১৮-appjs-ও-serverjs--সব-জোড়া-লাগানো)
20. [১৯. React থেকে Route ডাকা — CORS, fetch আর Deploy](#১৯-react-থেকে-route-ডাকা--cors-fetch-আর-deploy)
21. [২০. REST নকশার নিয়ম — Route-এর নাম কেমন হবে](#২০-rest-নকশার-নিয়ম--route-এর-নাম-কেমন-হবে)
22. [২১. Route টেস্ট করার উপায় — ব্রাউজার যথেষ্ট নয়](#২১-route-টেস্ট-করার-উপায়--ব্রাউজার-যথেষ্ট-নয়)
23. [২২. সাধারণ ভুল ও সমাধান — Route নিয়ে যেখানে সবাই হোঁচট খায়](#২২-সাধারণ-ভুল-ও-সমাধান--route-নিয়ে-যেখানে-সবাই-হোঁচট-খায়)
24. [২৩. সারসংক্ষেপ ও Practice আইডিয়া](#২৩-সারসংক্ষেপ-ও-practice-আইডিয়া)
25. [২৪. প্রশ্ন ও উত্তর (Q&A)](#২৪-প্রশ্ন-ও-উত্তর-qa)
26. [✅ কোনটা কীভাবে যাচাই করেছি — একটা সৎ নোট](#-কোনটা-কীভাবে-যাচাই-করেছি--একটা-সৎ-নোট)


---

## ভূমিকা: স্বপ্ননগর মলের গল্প

কল্পনা করুন, শহরের মাঝখানে **স্বপ্ননগর মল** নামে এক বিশাল শপিং মল। পাঁচ তলা, শত শত দোকান। একদিন **তিথি** নামের এক ক্রেতা প্রথমবার মলে ঢুকলো। ভেতরে ঢুকে সে ঘাবড়ে গেলো — কোথায় কী আছে কিছুই জানে না। ঠিক তখনই গেটের পাশে তার চোখ পড়লো একটা বড় **ডিরেক্টরি বোর্ডে**:

```
┌───────────────────┬──────────────────────────┬────────────────────┐
│ কোথায় (ঠিকানা)   │ কী কাজ করা যায়          │ কে করে দেবে        │
├───────────────────┼──────────────────────────┼────────────────────┤
│ ৩ তলা / বইঘর      │ সব বইয়ের তালিকা দেখা     │ বিক্রয়কর্মী ১     │
│ ৩ তলা / বইঘর      │ নতুন বই জমা দেওয়া        │ বিক্রয়কর্মী ২     │
│ ৩ তলা / বইঘর / ৭  │ ৭ নম্বর বইটা দেখা         │ বিক্রয়কর্মী ৩     │
│ ৩ তলা / বইঘর / ৭  │ ৭ নম্বর বইয়ের দাম বদলানো │ বিক্রয়কর্মী ৪     │
│ ৩ তলা / বইঘর / ৭  │ ৭ নম্বর বই সরিয়ে ফেলা    │ বিক্রয়কর্মী ৫     │
│ ১ তলা / সদস্য ডেস্ক │ নতুন সদস্য হওয়া          │ ডেস্ক কর্মী        │
└───────────────────┴──────────────────────────┴────────────────────┘
```

তিথি বুঝে ফেললো — মলে কিছু চাইতে হলে দুটো জিনিস বলতে হয়:

1. **কোথায়?** — কোন তলা, কোন দোকান (এটা **Path**)
2. **কী কাজ?** — দেখবো, জমা দেবো, বদলাবো, ফেলবো (এটা **HTTP Method**)

এই **ঠিকানা + কাজের জোড়া**টাই হলো একটা **Route**। আর পুরো বোর্ডটা হলো **Route Table**। আর প্রতিটা সারির শেষে যে কর্মী কাজটা করে দেন, তিনি হলেন **Handler**।

এবার তিথি যখন মলের কর্মীকে বললো, "৩ তলার বইঘরে গিয়ে ৭ নম্বর বইটা দেখাও", তখন তিনটা জিনিসের যেকোনো একটা ঘটতে পারে:

| কী ঘটলো | গল্পে | Express-এ |
|---|---|---|
| ঠিকানা আর কাজ দুটোই বোর্ডে আছে | সংশ্লিষ্ট কর্মী কাজটা করে দিলেন | Route মিললো → Handler চললো → **200 / 201** |
| বোর্ডে এই নামে কোনো ঠিকানাই নেই | "দুঃখিত, এই নামে কোনো দোকান নেই" | কোনো Route মেলেনি → **404 Not Found** |
| কর্মী কাজ করতে গিয়ে বিপদে পড়লেন (গুদামের তালা ভাঙা!) | অভিযোগ ডেস্ক ব্যাপারটা সামলায় | Handler-এ error → Error Handler → **500** |

### গল্পের চরিত্রগুলো কে কে?

| গল্পে | MERN-এ |
|---|---|
| স্বপ্ননগর মল (পুরো বিল্ডিং) | Express সার্ভার (`app`) |
| ক্রেতা তিথি ও তার মোবাইল অ্যাপ | Client — React অ্যাপ / Browser / Postman |
| ডিরেক্টরি বোর্ড | Route Table |
| তলার ম্যানেজার | `express.Router()` |
| গেটের সিকিউরিটি | Middleware |
| বিক্রয়কর্মী | Controller / Handler |
| গুদাম | MongoDB (Mongoose দিয়ে) |
| সদস্য কার্ড | JWT Token |
| অভিযোগ ডেস্ক | Error-handling Middleware |

> **MERN-এর চার অক্ষর:** **M**ongoDB = গুদাম, **E**xpress = মলের ব্যবস্থাপনা (**Route এখানেই থাকে**), **R**eact = ক্রেতার হাতের অ্যাপ, **N**ode.js = যে বিদ্যুতে পুরো মল চলে (runtime)।

```mermaid
flowchart LR
    U["👤 ক্রেতা<br/>React অ্যাপ / Browser / Postman"] -- "HTTP Request<br/>GET /api/v1/books" --> R["🛎️ Express<br/>Route মেলানো"]
    R --> M["🚧 Middleware<br/>সিকিউরিটি গেট"]
    M --> C["🧑‍💼 Controller<br/>বিক্রয়কর্মী"]
    C --> D["📦 Mongoose Model"]
    D --> DB[("🗄️ MongoDB<br/>গুদাম")]
    DB --> D
    D --> C
    C -- "JSON Response<br/>200 OK + বইয়ের তালিকা" --> U
```

আজকের গল্পে আমরা **BookNest** নামে একটা বইয়ের দোকানের Backend বানাবো — যেখানে বই যোগ/দেখা/বদলানো/মোছা যাবে, সদস্য হওয়া ও লগইন করা যাবে, আর বইয়ে রিভিউ দেওয়া যাবে। এই ছোট প্রজেক্টেই Route-এর প্রায় সব কিছু এসে যাবে।

```mermaid
flowchart LR
    A["১-২<br/>Route কী আর<br/>Frontend থেকে আলাদা কোথায়"] --> B["৩-৭<br/>Method, Path, Params,<br/>Query, Body, Response"]
    B --> C["৮-১১<br/>next, ক্রম, Router,<br/>Controller"]
    C --> D["১২<br/>Middleware:<br/>Auth ও Role"]
    D --> E["১৩-১৮<br/>পুরো BookNest API<br/>MongoDB সহ"]
    E --> F["১৯-২১<br/>React থেকে ডাকা,<br/>REST নিয়ম, টেস্ট"]
    F --> G["২২-২৪<br/>ভুল, সারসংক্ষেপ,<br/>প্রশ্ন ও উত্তর"]
```

---

## ১. Route কাকে বলে — তিনটা উপাদান

### গল্প

ডিরেক্টরি বোর্ডের প্রতিটা সারি আসলে তিনটা তথ্যের সমষ্টি — **কাজের ধরন**, **ঠিকানা** আর **কর্মী**। Express-এ একটা Route লেখার সময়ও হুবহু এই তিনটা জিনিস লিখতে হয়।

### গঠন (Anatomy)

```
app . get ( "/books/:id" , (req, res) => { res.json(...) } )
 │     │         │                      │
 │     │         │                      └── ৩. Handler  — কে কাজটা করবে (কর্মী)
 │     │         └── ২. Path    — কোন ঠিকানায় (কোন দোকান)
 │     └── ১. Method  — কী ধরনের কাজ (দেখা / জমা / বদলানো / ফেলা)
 └── কে রাখছে — app নিজে, অথবা একটা Router (তলার ম্যানেজার)
```

```javascript
app.get("/books", (req, res) => {
  res.json({ message: "এটা সব বইয়ের তালিকা" });
});
```

| উপাদান | উদাহরণ | মানে |
|---|---|---|
| Method | `app.get` | শুধু **GET** কাজের জন্য। `app.post`, `app.put`, `app.patch`, `app.delete`ও আছে |
| Path | `"/books"` | URL-এর পথ অংশ (domain, port আর `?` এর পরের query বাদে) |
| Handler | `(req, res) => {...}` | Route মিললে এই ফাংশন চলে। `req` = ক্রেতার আনা খাম, `res` = ফেরত পাঠানোর ট্রে |

> **নিয়ম:** Route মিলতে হলে **Method আর Path — দুটোই** মিলতে হবে। `GET /books` আর `POST /books` দুটো সম্পূর্ণ আলাদা Route, এদের কর্মীও আলাদা।

### একটা Route-এ Handler একটা নয়, একাধিকও হতে পারে

```javascript
app.get("/books", checkSomething, anotherCheck, (req, res) => {
  // checkSomething আর anotherCheck হলো Middleware (৮ ও ১২ নম্বর সেকশনে বিস্তারিত)
  // সবার শেষের ফাংশনটাই সাধারণত আসল কাজের কর্মী
});
```

### সব Route মিলে যা হয়

Express ভেতরে ভেতরে Route গুলোকে একটা **তালিকায় (stack)** ওপর থেকে নিচে সাজিয়ে রাখে। Request এলে সেই তালিকা ধরে ওপর থেকে একে একে মিলিয়ে দেখে। এই "ওপর থেকে নিচে" ব্যাপারটা পরে (৮ নম্বর সেকশনে) খুব গুরুত্বপূর্ণ হয়ে দাঁড়াবে।

```mermaid
flowchart TD
    A["📨 Request এলো<br/>যেমন PATCH /api/v1/books/7"] --> B["তালিকার প্রথম layer থেকে শুরু"]
    B --> C{"Method আর Path<br/>দুটোই মিলেছে?"}
    C -- "না" --> D{"আরো layer বাকি আছে?"}
    D -- "হ্যাঁ" --> B
    D -- "না" --> E["🚫 404 Not Found"]
    C -- "হ্যাঁ" --> F["Handler চালাও"]
    F --> G{"Handler কী করলো?"}
    G -- "res.json / res.send" --> H["✅ উত্তর গেল, যাত্রা শেষ"]
    G -- "next()" --> D
    G -- "next(err) বা throw" --> I["🛑 সোজা Error Handler-এ"]
```

### WordPress/PHP থেকে এলে — তুলনা

আপনি যদি WordPress REST API-তে কাজ করে থাকেন, তাহলে এই ধারণাগুলো আগে থেকেই চেনা লাগবে। শুধু নামগুলো আলাদা:

| Express (Node.js) | WordPress REST API (PHP) |
|---|---|
| `app.use("/api/v1", routes)` | Namespace: `/wp-json/booknest/v1/` |
| `app.get("/books/:id", handler)` | `register_rest_route( 'booknest/v1', '/books/(?P<id>[a-f0-9]{24})', [ 'methods' => 'GET', 'callback' => ... ] )` |
| `protect`, `restrictTo("admin")` middleware | `permission_callback` |
| `req.params`, `req.query`, `req.body` | `$request->get_param('id')`, `get_query_params()`, `get_json_params()` |
| `res.status(201).json(data)` | `new WP_REST_Response( $data, 201 )` |
| `throw new ApiError(404, "...")` | `new WP_Error( 'not_found', '...', [ 'status' => 404 ] )` |
| `express.Router()` (ফিচার অনুযায়ী ভাগ) | আলাদা Controller class (`WP_REST_Controller` বাড়িয়ে) |

মূল পার্থক্য: WordPress-এ Route-এর regex সরাসরি path-এ লেখা হয়, Express-এ Route Parameter (`:id`) আর আলাদা validation middleware ব্যবহার করা হয় (৫ ও ১০ নম্বর সেকশনে দেখবো)।

---

## ২. Frontend Route বনাম Backend Route

### গল্প

তিথির হাতের মোবাইল অ্যাপে (React) একটা **"ম্যাপ"** আছে। ম্যাপে সে আঙুল দিয়ে "৩ তলা" ট্যাপ করলে স্ক্রিন বদলে ৩ তলার ছবি দেখায় — কিন্তু এতে সে কিন্তু **মলের ভেতরে কাউকে জিজ্ঞেসই করেনি**, অ্যাপ নিজেই স্ক্রিন বদলেছে। আর যখন সে অ্যাপ থেকে বলে, "আমাকে বইয়ের আসল তালিকা এনে দাও" — তখন অ্যাপ **মলের কর্মীর কাছে** যায়।

MERN-এ **দুই ধরনের Route** আছে, আর নতুনদের সবচেয়ে বেশি গুলিয়ে যায় এখানেই:

| | Frontend Route (React Router) | Backend Route (Express) |
|---|---|---|
| কোথায় চলে | ব্রাউজারে (ক্রেতার ফোনে) | সার্ভারে (মলের ভেতরে) |
| কাজ | কোন **পেজ/স্ক্রিন** দেখাবে ঠিক করা | কোন **ডেটা/কাজ** করবে ঠিক করা |
| উদাহরণ | `/books`, `/login`, `/dashboard` | `/api/v1/books`, `/api/v1/auth/login` |
| লাইব্রেরি | `react-router-dom` | `express` |
| উত্তরে কী দেয় | JSX / HTML স্ক্রিন | সাধারণত JSON |
| সার্ভারে request যায়? | **না** (URL বদলায়, কিন্তু সার্ভারে যায় না) | **হ্যাঁ** |

### একই ঠিকানা, দুই অর্থ — সমস্যা কোথায়?

ধরুন React-এ একটা পেজ আছে `/books` (বইয়ের তালিকা দেখানোর স্ক্রিন) আর Backend-এও একটা Route আছে `/books` (বইয়ের ডেটা দেওয়ার)। ব্যবহারকারী ব্রাউজারে `/books` লিখে **Refresh** দিলে ব্রাউজার সরাসরি **সার্ভারে GET /books** পাঠায় — তখন সে স্ক্রিন না পেয়ে কাঁচা JSON পেয়ে যাবে!

এই কারণেই Backend-এর সব Route **`/api`** দিয়ে শুরু করা হয়: `/api/v1/books`। তাহলে `/books` পড়ে থাকে React-এর জন্য, আর `/api/...` Express-এর জন্য।

```mermaid
sequenceDiagram
    autonumber
    participant U as 👤 ব্যবহারকারী
    participant R as ⚛️ React অ্যাপ (Browser)
    participant E as 🛎️ Express (Backend)
    U->>R: মেনুতে "Books" ট্যাপ করলো
    R->>R: React Router নিজেই /books পেজ দেখালো (সার্ভারে কিছু যায়নি)
    R->>E: fetch("/api/v1/books") — আসল ডেটা চাই
    E->>E: Route মিলিয়ে Handler চালালো
    E-->>R: 200 OK + JSON বইয়ের তালিকা
    R-->>U: তালিকা স্ক্রিনে আঁকলো
```

> **মনে রাখার সূত্র:** *React Router = "কোন স্ক্রিন?"* আর *Express Route = "কোন ডেটা / কোন কাজ?"* এই ফাইলে আমরা শুধু দ্বিতীয়টা নিয়ে কথা বলছি। React থেকে Backend Route কীভাবে ডাকতে হয়, সেটা ১৯ নম্বর সেকশনে আছে।

### ব্রাউজারের Address Bar সবসময় GET পাঠায়

Address Bar-এ কোনো ঠিকানা লিখে Enter চাপলে বা কোনো লিংকে ক্লিক করলে **সবসময় GET** যায়। POST, PUT, PATCH, DELETE পাঠাতে হলে লাগবে `fetch()`/`axios`, ফর্ম, অথবা Postman। তাই Route টেস্টের জন্য শুধু ব্রাউজার যথেষ্ট নয় (২১ নম্বর সেকশনে টেস্টের উপায় আছে)।

---

## ৩. প্রজেক্ট সেটআপ ও Express 5

### গল্প

মলের পুরো নকশায় যাওয়ার আগে চলুন একটা ছোট **খেলার মাঠ** বানাই — যেখানে গুদাম (MongoDB) নেই, বইয়ের তালিকা শুধু একটা array। এতে Route-এর ধারণাগুলো কোনো ঝামেলা ছাড়া হাতে-কলমে শেখা যাবে। ১৩ নম্বর সেকশন থেকে আসল MongoDB-র প্রজেক্টে যাবো।

### টার্মিনালে কমান্ড

```bash
mkdir booknest-api && cd booknest-api   # প্রজেক্ট ফোল্ডার বানিয়ে ভেতরে ঢোকা
npm init -y                              # package.json বানানো (-y = সব প্রশ্নের ডিফল্ট উত্তর)
npm install express@5 mongoose dotenv cors jsonwebtoken bcryptjs
```

| প্যাকেজ | গল্পে কাজ | কেন লাগে |
|---|---|---|
| `express` | মলের ব্যবস্থাপনা | Route, Middleware, Router — সব এখান থেকে |
| `mongoose` | গুদামের সাথে কথা বলার অনুবাদক | MongoDB-তে ডেটা রাখা/আনা (১৩ নম্বর থেকে) |
| `dotenv` | গোপন চাবির সিন্দুক | `.env` ফাইলের মান `process.env`-এ আনে |
| `cors` | অন্য মলের ক্রেতাকে ঢোকার অনুমতিপত্র | React (আলাদা port) থেকে request ঢুকতে দেয় (১৯ নম্বর) |
| `jsonwebtoken` | সদস্য কার্ড ছাপানোর মেশিন | Login-এর পর JWT token বানানো ও যাচাই |
| `bcryptjs` | password-এর তালা | password সরাসরি না রেখে hash করে রাখা |

`package.json`-এর `scripts` অংশে:

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "node --watch server.js"
  }
}
```

> `node --watch` = ফাইল সেভ করলেই সার্ভার নিজে থেকে restart হবে (Node 18+ এ বিল্ট-ইন)। আলাদা `nodemon` লাগে না।

### Express 4 আর Express 5 — Route-এর ক্ষেত্রে যা যা বদলেছে

এখন `npm install express` দিলে **Express 5** আসে। কিন্তু ইন্টারনেটের অনেক পুরোনো টিউটোরিয়ালে Express 4-এর কোড। Route-সংক্রান্ত এই পার্থক্যগুলো না জানলে সেই কোড কপি করলেই error খাবেন:

| বিষয় | Express 4 | Express 5 |
|---|---|---|
| সব-ধরা (wildcard) | `app.get("*")`, `"/files/*"` | নাম দিতে হয়: `"/files/*splat"`। `req.params.splat` হয় একটা **array** |
| root সহ সব-ধরা | `"*"` | `"/{*splat}"` |
| ঐচ্ছিক (optional) অংশ | `"/books/:id?"` | `"/books{/:id}"` (মাঝারি বন্ধনী `{}`) |
| Path-এর ভেতরে regex | `"/:id(\\d+)"` | ❌ সরানো হয়েছে — Route তৈরির সময়ই error |
| `async` handler-এ error | নিজে `try/catch` বা wrapper লাগতো | নিজে থেকেই Error Handler-এ চলে যায় |
| `req.body` (parser না থাকলে) | `{}` | `undefined` |
| Query parser (ডিফল্ট) | `extended` (nested হতো) | `simple` (`?a[b]=1` nested হয় না) |
| `app.del()` | ছিল | সরানো — `app.delete()` ব্যবহার করুন |
| `req.param("name")` | ছিল (deprecated) | সরানো — `req.params`, `req.query`, `req.body` |
| `res.json(obj, status)` ধরনের পুরোনো লেখা | deprecated | সরানো — `res.status(x).json(obj)` |

> **Express 5-এর Route ভাঙলে কী error আসে?** `app.get("*", ...)` লিখলে সার্ভার চালু হওয়ার সময়ই `Missing parameter name at index 1: *` ধরনের error দেয়। মানে ভুল Route ধরা পড়ে **শুরুতেই**, রাতে গিয়ে নয়।

### খেলার মাঠের ফাইল — `playground.js`

এটাই আমাদের শুরুর কাঠামো। পরের সেকশনগুলোর Route গুলো এখানে `// Route গুলো এখানে বসবে` জায়গায় একে একে যোগ করে চালিয়ে দেখবেন।

```javascript
// playground.js — DB ছাড়া Route শেখার খেলার মাঠ

const express = require("express");
// express = মলের ব্যবস্থাপনার যন্ত্র। Route, Middleware, Router সব এর ভেতরে

const app = express();
// app = আমাদের পুরো Express অ্যাপ্লিকেশন। সব Route এর গায়েই লাগবে

const PORT = 3000;

app.use(express.json());
// express.json() = ক্রেতা JSON body পাঠালে সেটা পড়ে req.body-তে বসিয়ে দেয় (POST/PUT/PATCH-এ লাগবে)
// ⚠️ এটা অবশ্যই Route গুলোর ওপরে, নাহলে Route চলার সময় req.body পাওয়া যাবে না

let nextId = 5;
// nextId = পরের নতুন বইয়ের id। let, কারণ প্রতিবার বাড়াবো

const books = [
  { id: 1, title: "পথের পাঁচালী", author: "বিভূতিভূষণ বন্দ্যোপাধ্যায়", price: 250, category: "novel" },
  { id: 2, title: "গীতাঞ্জলি", author: "রবীন্দ্রনাথ ঠাকুর", price: 180, category: "poetry" },
  { id: 3, title: "অগ্নিবীণা", author: "কাজী নজরুল ইসলাম", price: 150, category: "poetry" },
  { id: 4, title: "দেবদাস", author: "শরৎচন্দ্র চট্টোপাধ্যায়", price: 120, category: "novel" },
];
// books = নকল ডেটাবেস। ⚠️ এটা RAM-এ থাকে, সার্ভার restart দিলে নতুন যোগ করা সব বই উধাও

// ---------- Route গুলো এখানে বসবে ----------

app.listen(PORT, () => {
  console.log(`✅ খেলার মাঠ চালু → http://localhost:${PORT}`);
});
```

চালানোর কমান্ড: `node --watch playground.js`

---

## ৪. HTTP Method — কাজের ধরন

### গল্প

তিথি বইঘরে গিয়ে পাঁচ রকম কথা বলতে পারে:

- **"বইগুলো দেখান"** → শুধু দেখা, দোকানের কিছু বদলায় না → **GET**
- **"এই নতুন বইটা জমা নিন"** → নতুন কিছু তৈরি হলো → **POST**
- **"৭ নম্বর বইয়ের পুরো তথ্য নতুন করে লিখে দিন"** → পুরোনোটা সম্পূর্ণ বদলে নতুন → **PUT**
- **"৭ নম্বর বইয়ের শুধু দামটা বদলে দিন"** → বাকি সব যেমন ছিল তেমন, শুধু একটা জিনিস → **PATCH**
- **"৭ নম্বর বইটা সরিয়ে ফেলুন"** → মুছে ফেলা → **DELETE**

এই পাঁচটাকে একসাথে বলে **CRUD** (Create, Read, Update, Delete) — প্রায় সব অ্যাপের Backend-এর মূল কাজ।

### এক নজরে সব Method

| Method | CRUD | কাজ | Mongoose-এ সাধারণত | সফল হলে Status | Body লাগে? |
|---|---|---|---|---|---|
| `GET` | Read | ডেটা দেখা | `find()`, `findById()` | `200` | না |
| `POST` | Create | নতুন কিছু তৈরি | `create()` | `201` | হ্যাঁ |
| `PUT` | Update (পুরোটা) | সম্পূর্ণ প্রতিস্থাপন | `set()` + `save()` | `200` | হ্যাঁ (সব ফিল্ড) |
| `PATCH` | Update (আংশিক) | কিছু ফিল্ড বদলানো | `set()` + `save()` বা `findByIdAndUpdate()` | `200` | হ্যাঁ (শুধু বদলানো ফিল্ড) |
| `DELETE` | Delete | মুছে ফেলা | `deleteOne()` | `204` (বা `200`) | সাধারণত না |

### Safe আর Idempotent — দুটো গুরুত্বপূর্ণ শব্দ

- **Safe** = কাজটা করলে সার্ভারের কিছু **বদলায় না**। (GET, HEAD, OPTIONS)
- **Idempotent** = একই কাজ **একবার করলে যা ফল, দশবার করলেও সেই একই ফল**।

গল্পে: "৭ নম্বর বইয়ের দাম ২০০ করে দিন" — দশবার বললেও দাম ২০০-ই থাকে (**PUT idempotent**)। "৭ নম্বর বই সরান" — প্রথমবারে সরে যায়, পরের ন'বার "বইটা নেই" বলে, কিন্তু মলের অবস্থা একই (**DELETE idempotent**)। কিন্তু "নতুন বই জমা নিন" দশবার বললে **দশটা** নতুন বই তৈরি হয়ে যাবে (**POST idempotent নয়**)।

| Method | Safe? | Idempotent? |
|---|---|---|
| GET | ✅ | ✅ |
| POST | ❌ | ❌ |
| PUT | ❌ | ✅ |
| PATCH | ❌ | সাধারণত ❌ (`"দাম +১০ করো"` ধরনের হলে ফল বদলায়) |
| DELETE | ❌ | ✅ |

> কেন জরুরি? নেটওয়ার্ক ধীর থাকলে ব্রাউজার/অ্যাপ অনেক সময় একই request আবার পাঠায়। Idempotent Method-এ সেটা নিরাপদ; POST-এ দুবার গেলে দুটো অর্ডার তৈরি হতে পারে।

### কোড — পাঁচ Method-এর পাঁচ Route (খেলার মাঠে বসান)

```javascript
const findIndexById = (id) => books.findIndex((book) => book.id === Number(id));
// findIndexById = id দিয়ে বইটা array-র কত নম্বর জায়গায় আছে খোঁজে। না পেলে -1
// Number(id) কারণ req.params.id সবসময় String আসে ("3"), আর আমাদের book.id হলো সংখ্যা (3)

// 1️⃣ GET — সব বই দেখা
app.get("/books", (req, res) => {
  res.json(books);
  // res.json() = JS array/object-কে JSON বানিয়ে পাঠায় + Content-Type: application/json নিজে বসায়
});

// 2️⃣ GET — একটা বই দেখা
app.get("/books/:id", (req, res) => {
  const index = findIndexById(req.params.id);
  // :id = "এখানে যা-ই আসুক আমি ওটাকে id নামে ধরে রাখবো" (Route Parameter — ৫ নম্বর সেকশনে বিস্তারিত)
  if (index === -1) {
    return res.status(404).json({ error: "বই পাওয়া যায়নি।" });
    // return লিখেছি যাতে নিচের res.json() আর না চলে (একটা request-এ উত্তর একবারই পাঠানো যায়)
  }
  res.json(books[index]);
});

// 3️⃣ POST — নতুন বই জমা
app.post("/books", (req, res) => {
  const { title, author, price, category } = req.body ?? {};
  // req.body = ক্রেতার পাঠানো JSON। ?? {} কারণ Express 5-এ body না এলে req.body হয় undefined

  if (!title || !author || typeof price !== "number") {
    return res.status(400).json({ error: "title, author (লেখা) আর price (সংখ্যা) দিতে হবে।" });
    // 400 Bad Request = ক্রেতার পাঠানো তথ্যে ভুল
  }

  const book = { id: nextId++, title, author, price, category: category ?? "novel" };
  // nextId++ = বর্তমান মান ব্যবহার করে তারপর ১ বাড়ায়। id নিজে বানাই, ক্রেতার কাছ থেকে নিই না
  books.push(book);

  res.status(201).json(book);
  // 201 Created = "নতুন জিনিস তৈরি হয়েছে"। সাথে নতুন বইটা ফেরত দিলাম, যাতে ক্রেতা নতুন id-টা জানতে পারে
});

// 4️⃣ PUT — পুরো বই প্রতিস্থাপন
app.put("/books/:id", (req, res) => {
  const index = findIndexById(req.params.id);
  if (index === -1) return res.status(404).json({ error: "বই পাওয়া যায়নি।" });

  const { title, author, price, category } = req.body ?? {};
  if (!title || !author || typeof price !== "number") {
    return res.status(400).json({ error: "PUT-এ title, author, price তিনটাই দিতে হবে।" });
  }

  books[index] = { id: books[index].id, title, author, price, category: category ?? "novel" };
  // পুরোনো বইয়ের জায়গায় সম্পূর্ণ নতুন object বসালাম (শুধু id আগেরটা রাখলাম)
  // body-তে যে ফিল্ড নেই, সেটা আগের মান নয় — ডিফল্টে চলে গেল। এটাই PUT-এর "পুরোটা প্রতিস্থাপন"

  res.json(books[index]);
});

// 5️⃣ PATCH — শুধু পাঠানো ফিল্ডগুলো বদলানো
app.patch("/books/:id", (req, res) => {
  const index = findIndexById(req.params.id);
  if (index === -1) return res.status(404).json({ error: "বই পাওয়া যায়নি।" });

  const allowed = ["title", "author", "price", "category"];
  // allowed = ক্রেতা যে ফিল্ডগুলো বদলাতে পারবে (সাদা তালিকা)। id বদলানোর সুযোগ দিইনি

  for (const field of allowed) {
    if (req.body?.[field] !== undefined) {
      books[index][field] = req.body[field];
      // শুধু যে ফিল্ড body-তে এসেছে সেটাই বদলালো, বাকিগুলো যেমন ছিল তেমন
    }
  }

  res.json(books[index]);
});

// 6️⃣ DELETE — বই মোছা
app.delete("/books/:id", (req, res) => {
  const index = findIndexById(req.params.id);
  if (index === -1) return res.status(404).json({ error: "বই পাওয়া যায়নি।" });

  books.splice(index, 1);
  // splice(কোথা থেকে, কয়টা) = array থেকে ওই জায়গার ১টা জিনিস কেটে বাদ দেয়

  res.status(204).end();
  // 204 No Content = "কাজ হয়ে গেছে, ফেরত দেওয়ার মতো কিছু নেই" — তাই body ছাড়া .end()
});
```

### টেস্ট করার তালিকা (Postman / Thunder Client-এ)

| Method | URL | Body (JSON) | আশা করা উত্তর |
|---|---|---|---|
| GET | `/books` | — | `200` + ৪টা বই |
| GET | `/books/2` | — | `200` + গীতাঞ্জলি |
| GET | `/books/99` | — | `404` |
| POST | `/books` | `{"title":"শেষের কবিতা","author":"রবীন্দ্রনাথ ঠাকুর","price":200}` | `201` + `id: 5` |
| POST | `/books` | `{"title":"শুধু নাম"}` | `400` |
| PUT | `/books/5` | `{"title":"শেষের কবিতা","author":"রবীন্দ্রনাথ ঠাকুর","price":220}` | `200` |
| PATCH | `/books/5` | `{"price":250}` | `200`, শুধু দাম বদলেছে |
| DELETE | `/books/5` | — | `204` (আবার দিলে `404`) |

### PUT বনাম PATCH — পার্থক্যটা হাতে-কলমে

বইটা এমন: `{ title: "গীতাঞ্জলি", author: "রবীন্দ্রনাথ", price: 180, category: "poetry" }`

| কী পাঠালেন | `PUT` করলে ফল | `PATCH` করলে ফল |
|---|---|---|
| `{ "price": 200 }` | ❌ `400` (কারণ title, author নেই) | ✅ শুধু price ২০০, বাকি সব আগের মতো |
| `{ "title": "গীতাঞ্জলি", "author": "রবীন্দ্রনাথ", "price": 200 }` | ✅ সফল, কিন্তু `category` **`poetry` থেকে ডিফল্ট `novel`-এ ফিরে গেল** (কারণ পাঠাইনি) | ✅ price ২০০ হলো, `category` আগের মতোই `poetry` |

> **কোনটা কখন?** ফর্মে সব ঘর নতুন করে ভরে "Save" চাপলে → PUT। শুধু একটা toggle বা একটা ঘর বদলালে (যেমন "স্টকে আছে" টিক চিহ্ন) → PATCH। বাস্তবে অনেক প্রজেক্টে শুধু PATCH দিয়েই কাজ চালানো হয়।

### আরও দুটো Method — HEAD আর OPTIONS

| Method | কাজ | Express-এ |
|---|---|---|
| `HEAD` | GET-এর মতোই, কিন্তু শুধু header ফেরত দেয়, body নয় (ফাইলের সাইজ বা আছে-কি-নেই দেখতে) | `app.get()` থাকলে HEAD **নিজে থেকেই** কাজ করে |
| `OPTIONS` | "এই ঠিকানায় কী কী করা যায়?" — উত্তরে `Allow` header আসে | Express নিজে থেকেই সাড়া দেয়। আর ব্রাউজার cross-origin request পাঠানোর আগে যে **preflight** OPTIONS পাঠায়, সেটা `cors` middleware সামলায় (১৯ নম্বর সেকশন) |

---

## ৫. Route Path — ঠিকানার ধরন

### গল্প

ডিরেক্টরি বোর্ডে সব ঠিকানা এক রকম নয়:

- **"৩ তলা / বইঘর"** — একদম নির্দিষ্ট, কখনো বদলায় না (**Static path**)
- **"৩ তলা / বইঘর / ⟨যেকোনো নম্বর⟩"** — নম্বরের জায়গায় যা-ই বসুক, একই কর্মী দেখবেন (**Route Parameter**)
- **"বইঘর / ⟨নম্বর⟩ / রিভিউ / ⟨নম্বর⟩"** — একাধিক ফাঁকা ঘর (**একাধিক Parameter**)
- **"গুদামের যেকোনো তাকের যেকোনো জিনিস"** — কতদূর গভীরে যাবে ঠিক নেই (**Wildcard**)

### একটা পুরো URL-এর কোন অংশের কী নাম

```
http://localhost:5000/api/v1/books/7?category=novel&page=2
```

| অংশ | উদাহরণ | মানে |
|---|---|---|
| Protocol | `http://` | কোন নিয়মে কথা হবে (নিরাপদটা `https://`) |
| Host | `localhost` | কোন কম্পিউটার (মলের ঠিকানা) |
| Port | `:5000` | ওই কম্পিউটারের কোন দরজা |
| **Path** | `/api/v1/books/7` | **Route মেলানো হয় শুধু এই অংশ দিয়ে** |
| Query String | `?category=novel&page=2` | `?` দিয়ে শুরু। Route মেলানোয় **ধরা হয় না**, আলাদাভাবে `req.query`-তে পাওয়া যায় |

### ১. Static path — নির্দিষ্ট ঠিকানা

```javascript
app.get("/books", (req, res) => res.send("সব বই"));
app.get("/books/latest", (req, res) => res.send("সর্বশেষ বই"));
// দুটো সম্পূর্ণ আলাদা Route। /books/latest কখনো /books-এর অংশ হিসেবে ধরা হয় না
```

### ২. Route Parameter — `:নাম`

```javascript
app.get("/books/:id", (req, res) => {
  res.json({ id: req.params.id });
  // /books/7 → { "id": "7" }। ⚠️ 7 কিন্তু সংখ্যা নয়, String "7"!
});
```

**একাধিক Parameter:**

```javascript
app.get("/authors/:authorId/books/:bookId", (req, res) => {
  res.json(req.params);
  // /authors/7/books/9 → { "authorId": "7", "bookId": "9" }
});
```

**Parameter-এর নিয়ম:**

- নামের আগে `:` দিতে হয়; নামে অক্ষর, সংখ্যা ও `_` চলে।
- মান সবসময় **String**। সংখ্যা দরকার হলে `Number(req.params.id)` করতে হবে।
- একটা Parameter শুধু **এক ধাপের** মান ধরে — `/` এর পরে থামে। মানে `/books/:id` কখনো `/books/7/reviews` মেলাবে না।
- URL-এ বাংলা বা স্পেস থাকলে ব্রাউজার `%E0%A6%AA...` আকারে পাঠায়; Express `req.params`-এ **নিজে ডিকোড করে** আসল লেখা দেয় (`/search/পথের%20পাঁচালী` → `"পথের পাঁচালী"`)।

### ৩. ঐচ্ছিক অংশ — `{ }` (Express 5)

```javascript
app.get("/books{/:id}", (req, res) => {
  res.json({ id: req.params.id ?? null });
  // /books   → { "id": null }   (id এলো না)
  // /books/5 → { "id": "5" }
});

app.get("/img/:name{.:ext}", (req, res) => {
  res.json(req.params);
  // /img/cover.png → { "name": "cover", "ext": "png" }
  // /img/cover     → { "name": "cover" }
});
```

> Express 4-এ এটা লেখা হতো `"/books/:id?"`। Express 5-এ `?` চিহ্ন আর কাজ করে না (Route তৈরির সময়ই error দেয়), তার বদলে `{ }`।

### ৪. Wildcard — `*নাম` (Express 5-এ নাম দিতে হয়)

```javascript
app.get("/files/*splat", (req, res) => {
  res.json({ parts: req.params.splat });
  // /files/a/b/c → { "parts": ["a", "b", "c"] }  ← array, কারণ / দিয়ে ভাগ হয়
  // ⚠️ /files একা এলে এই Route মিলবে না (কমপক্ষে একটা অংশ লাগে)
});

app.get("/downloads{/*splat}", (req, res) => {
  res.json({ parts: req.params.splat ?? [] });
  // {} দিয়ে ঐচ্ছিক বানালে /downloads একা এলেও মিলবে
});
```

`splat` নামটা আমাদের পছন্দমতো — `*rest`, `*path`ও চলে। কিন্তু **নাম থাকতেই হবে**; Express 4-এর খালি `*` এখন error।

### ৫. Regex — Express 5-এ path-এর ভেতরে নয়

Express 4-এ `"/books/:id(\\d+)"` লিখে বলা যেতো "id শুধু সংখ্যা হবে"। Express 5-এ এই সুবিধা **সরানো হয়েছে** (নিরাপত্তার কারণে — জটিল regex দিয়ে সার্ভার আটকে দেওয়া যেতো)। এখন যাচাইটা করতে হয় কোডে, আর সেটা আসলে বেশি পরিষ্কারও। ১০ নম্বর সেকশনে `router.param()` দিয়ে এটা কীভাবে করবো দেখাবো।

### Path মেলানোর ছোট ছোট নিয়ম

| নিয়ম | উদাহরণ |
|---|---|
| বড়-ছোট হাতের অক্ষর **ভেদ করে না** (ডিফল্টে) | `/Books` আর `/books` একই Route-এ যায় |
| শেষের `/` **ধরে না** (ডিফল্টে) | `/books/` আর `/books` একই |
| Query String Route মেলানোয় **বাদ** | `/books?page=2` মেলে `"/books"` Route-এ |
| ওপর থেকে নিচে, **প্রথম মিলটাই জেতে** | (৮ নম্বর সেকশনে বিস্তারিত) |

কড়া নিয়ম চাইলে: `app.set("case sensitive routing", true)` আর `app.set("strict routing", true)` — বা Router বানানোর সময় `express.Router({ caseSensitive: true, strict: true })`।

---

## ৬. Request-এর তথ্য — চার পকেট

### গল্প

তিথি কর্মীর কাছে যা যা নিয়ে আসে, তার সবই একটা খামে (**`req`**) ভরা। খামের **চারটা পকেট**:

| পকেট | গল্পে | উদাহরণ | Express-এ পড়ার উপায় |
|---|---|---|---|
| ১. URL-এর পথ | ঠিকানার ভেতরের নম্বর | `/books/7` | `req.params` |
| ২. URL Query | ঠিকানার শেষে জুড়ে দেওয়া চিরকুট | `?category=novel&page=2` | `req.query` |
| ৩. Header | খামের গায়ের লেবেল | `Authorization`, `Content-Type` | `req.get("...")` বা `req.headers` |
| ৪. Body | খামের ভেতরের চিঠি/পার্সেল | JSON, ফর্ম ডেটা, ছবি | `req.body`, `req.file` |

```mermaid
flowchart TB
    R["📨 একটা HTTP Request"] --> P1["পকেট ১ — ঠিকানার পথ<br/>/books/7"]
    R --> P2["পকেট ২ — ঠিকানার চিরকুট<br/>?category=novel"]
    R --> P3["পকেট ৩ — খামের লেবেল<br/>Headers"]
    R --> P4["পকেট ৪ — ভেতরের চিঠি<br/>Body"]
    P1 --> E1["req.params"]
    P2 --> E2["req.query"]
    P3 --> E3["req.get() বা req.headers"]
    P4 --> E4["req.body / req.file"]
```

> **মনে রাখার এক লাইন:** *Params = কোন জিনিস, Query = কীভাবে/কোন শর্তে, Header = কে আর কী ধরনের, Body = আসল মাল।*

### চার পকেট একসাথে দেখার Route

নিজের চোখে দেখতে একটা "প্রতিধ্বনি (echo)" Route বানিয়ে নিন:

```javascript
app.post("/echo/:id", (req, res) => {
  res.json({
    params: req.params,                     // পকেট ১
    query: req.query,                       // পকেট ২
    contentType: req.get("content-type"),   // পকেট ৩ (একটা header)
    body: req.body ?? null,                 // পকেট ৪
  });
});
```

Postman-এ `POST /echo/42?draft=true&tag=a&tag=b`, Header: `Content-Type: application/json`, Body: `{"note":"hi"}` পাঠালে উত্তর আসবে:

```json
{
  "params": { "id": "42" },
  "query": { "draft": "true", "tag": ["a", "b"] },
  "contentType": "application/json",
  "body": { "note": "hi" }
}
```

লক্ষ্য করুন — `draft` এর মান `"true"` **String** (Boolean নয়), আর `tag` দুইবার দেওয়ায় সেটা **array** হয়ে গেছে।

### Query নিয়ে সাবধানতা

```javascript
const page = Number(req.query.page) || 1;
// req.query.page সবসময় String (বা না দিলে undefined)। Number("abc") = NaN, আর NaN || 1 = 1 — ভুল বা খালি হলে ডিফল্ট ১

const inStock = req.query.inStock === "true";
// "true" লেখাটাকে Boolean বানাতে তুলনা করতে হয়। Boolean("false") কিন্তু true হয়ে যায়! (খালি নয় এমন যেকোনো String = true)

const categories = [].concat(req.query.category ?? []);
// একই key একাধিকবার এলে array, একবার এলে String, না এলে undefined। এই লাইনে সবসময় array পাওয়া যায়
```

### Body — তিন ধরন

| Content-Type | কী পাঠায় | কে পড়ে | Express-এ |
|---|---|---|---|
| `application/json` | React/fetch/axios-এর JSON | `express.json()` | `req.body` |
| `application/x-www-form-urlencoded` | সাধারণ HTML `<form>` | `express.urlencoded({ extended: true })` | `req.body` |
| `multipart/form-data` | ছবি/ফাইল সহ ফর্ম | **Multer** (আলাদা প্যাকেজ) | `req.body` + `req.file` |

> ফাইল আপলোড Route-এর ক্ষেত্রে Multer একটা **Route Middleware** হিসেবে বসে: `router.post("/", upload.single("cover"), createBook)`। Multer-এর পুরো গল্প আগের ফাইলে আছে; এখানে শুধু জানা দরকার যে এটা Route-এর ভেতরে Handler-এর আগে বসে (১২ নম্বর সেকশনে Route Middleware-এর ধারণা আসছে)।

### `req`-এর আরও কিছু কাজের জিনিস

| Property | উদাহরণ মান | কাজ |
|---|---|---|
| `req.method` | `"GET"` | কোন Method |
| `req.originalUrl` | `"/api/v1/books/7?x=1"` | পুরো ঠিকানা (query সহ, Router prefix সহ) |
| `req.path` | `"/books/7"` | query ছাড়া path |
| `req.baseUrl` | `"/api/v1"` | Router যে prefix-এ বসানো (১০ নম্বরে দেখবো) |
| `req.ip` | `"::1"` | ক্রেতার IP |
| `req.hostname` | `"localhost"` | কোন host-এ এসেছে |
| `req.protocol` | `"http"` | http নাকি https |
| `req.get("x-abc")` | | যেকোনো header (বড়-ছোট হাত ভেদ নেই) |

---

## ৭. Response ও Status Code — উত্তর পাঠানো

### গল্প

কর্মী কাজ শেষে ক্রেতাকে একটা **ট্রে (`res`)** ভরে উত্তর দেন। ট্রেতে তিনটা জিনিস থাকতে পারে: **স্লিপের ওপর ফলাফলের কোড (Status)**, **লেবেল (Header)** আর **আসল জিনিস (Body)**।

### `res`-এর গুরুত্বপূর্ণ মেথড

| মেথড | কাজ | উদাহরণ |
|---|---|---|
| `res.json()` | JSON পাঠায় (API-তে সবচেয়ে বেশি লাগে) | `res.json({ success: true })` |
| `res.send()` | লেখা/HTML পাঠায় | `res.send("হ্যালো")` |
| `res.status()` | Status code বসায় — **এরপর আরেকটা মেথড জুড়তে হয়** | `res.status(404).json({...})` |
| `res.end()` | body ছাড়া উত্তর শেষ করে | `res.status(204).end()` |
| `res.sendStatus()` | কোড আর তার ডিফল্ট লেখা একসাথে | `res.sendStatus(404)` → `Not Found` |
| `res.redirect()` | অন্য ঠিকানায় পাঠিয়ে দেয় | `res.redirect("/api/v1/books")` |
| `res.sendFile()` | একটা ফাইল পাঠায় (পূর্ণ path লাগে) | `res.sendFile(path.join(__dirname, "index.html"))` |
| `res.set()` | Response header বসায় | `res.set("Cache-Control", "no-store")` |

```javascript
res.status(201).json({ success: true, data: book });
// status() আগে, json() পরে — চেইন। status() না দিলে ডিফল্ট 200
```

> ⚠️ **একটা Request-এ উত্তর একবারই যায়।** দুবার `res.json()` চালালে `Cannot set headers after they are sent to the client` error আসে। তাই `if` এর ভেতরে উত্তর দিলে সামনে `return` লিখুন: `return res.status(404).json(...)`।

### Status Code — ফলাফলের কোড

কোডের প্রথম সংখ্যাটাই ঘটনার শ্রেণি বলে দেয়:

| শুরু | মানে | গল্পে |
|---|---|---|
| **2xx** | ✅ সফল | "কাজ হয়ে গেছে" |
| **3xx** | ↪️ অন্যত্র যান | "ওই দোকান এখন ৪ তলায়" |
| **4xx** | 🙋 ক্রেতার ভুল | "আপনার ফর্মে ভুল আছে / অনুমতি নেই" |
| **5xx** | 🔥 সার্ভারের ভুল | "আমাদের নিজেদের গুদামে আগুন!" |

**প্রতিদিনের কাজে যেগুলো লাগে:**

| কোড | নাম | কখন দেবেন | BookNest-এর উদাহরণ |
|---|---|---|---|
| `200` | OK | GET, PUT, PATCH সফল | বই দেখা, বই বদলানো |
| `201` | Created | POST-এ নতুন কিছু তৈরি | নতুন বই, নতুন সদস্য |
| `204` | No Content | সফল, কিন্তু ফেরত দেওয়ার কিছু নেই | বই মোছা |
| `400` | Bad Request | ক্রেতার পাঠানো তথ্যে ভুল | ভাঙা JSON, `price: "abc"`, ভুল ফরম্যাটের id |
| `401` | Unauthorized | **"আপনি কে, চিনতে পারছি না"** — লগইন নেই, token ভুল/মেয়াদোত্তীর্ণ | token ছাড়া বই যোগ করতে চাইলে |
| `403` | Forbidden | **"আপনাকে চিনি, কিন্তু এই কাজের অনুমতি নেই"** | সাধারণ সদস্য অন্যের বই মুছতে চাইলে |
| `404` | Not Found | জিনিস বা Route পাওয়া যায়নি | `/books/<নেই এমন id>` |
| `409` | Conflict | বর্তমান অবস্থার সাথে দ্বন্দ্ব | একই email দিয়ে আবার রেজিস্টার, একই বইয়ে দ্বিতীয় রিভিউ |
| `422` | Unprocessable Entity | ফরম্যাট ঠিক কিন্তু অর্থ ভুল (অনেকে validation error-এ ব্যবহার করে) | আমরা এই প্রজেক্টে `400`-ই ব্যবহার করবো |
| `429` | Too Many Requests | rate limit পার হলে | এক মিনিটে ১০০ বারের বেশি login চেষ্টা |
| `500` | Internal Server Error | সার্ভারের নিজের অপ্রত্যাশিত ভুল | কোডে bug, DB বন্ধ |

> **৪০১ বনাম ৪০৩ — গেটের গল্প:** সিকিউরিটি গার্ড যদি বলে "সদস্য কার্ড দেখান" আর আপনি দেখাতে পারলেন না — সেটা **401**। কার্ড দেখালেন, গার্ড চিনলো, কিন্তু বললো "এটা VIP লাউঞ্জ, শুধু গোল্ড সদস্যদের জন্য" — সেটা **403**।

### সবসময় একই ছাঁচে উত্তর দিন

Frontend যাঁরা লিখবেন, তাঁদের সুবিধার জন্য সব উত্তর একই আকারে দেওয়া ভালো। এই ফাইলে আমরা এই ছাঁচ মেনে চলবো:

```javascript
// ✅ সফল হলে
{ "success": true, "data": { ... } }

// ✅ তালিকা হলে (পেজিনেশন সহ)
{ "success": true, "page": 1, "limit": 10, "total": 23, "totalPages": 3, "data": [ ... ] }

// ❌ ব্যর্থ হলে
{ "success": false, "message": "এই বই পাওয়া যায়নি।" }
```

---

## ৮. Handler, `next()` ও Route-এর ক্রম

### গল্প

ডিরেক্টরি বোর্ড **ওপর থেকে নিচে** পড়া হয়। যে সারিতে ঠিকানা আর কাজ মিলে যায়, সেই কর্মীকে ডাকা হয়। কর্মী তিনটা কাজের একটা করতে পারেন:

1. **নিজেই কাজ শেষ করে ক্রেতাকে উত্তর দিয়ে দেন** (`res.json(...)`) → যাত্রা এখানেই শেষ।
2. **"এটা আমার একার কাজ না, পরের জনের কাছে যান" বলে ঠেলে দেন** (`next()`) → পরের মিলে-যাওয়া Handler চলে।
3. **"সর্বনাশ, গোলমাল হয়েছে!" বলে অভিযোগ ডেস্কে পাঠান** (`next(err)` বা `throw`) → সোজা Error Handler-এ।

আর যদি কর্মী **কিছুই না করেন** (উত্তরও দিলেন না, `next()`-ও ডাকলেন না)? তিথি কাউন্টারে দাঁড়িয়েই থাকবে — ব্রাউজারে লোডিং ঘুরতেই থাকবে। এটা নতুনদের খুব পরিচিত একটা bug।

### একটা Route-এ একাধিক Handler

```javascript
const checkA = (req, res, next) => {
  console.log("A পার হলো");
  next();
  // next() = "আমার কাজ শেষ, পরের Handler-এ যাও"
};

const checkB = (req, res, next) => {
  console.log("B পার হলো");
  next();
};

app.get("/demo", checkA, checkB, (req, res) => {
  res.send("সব পার হয়ে আসল কর্মীর কাছে পৌঁছালাম");
});
// একটা array-তেও দেওয়া যায়: app.get("/demo", [checkA, checkB], handler)
// টার্মিনালে দেখা যাবে: A পার হলো → B পার হলো, তারপর উত্তর
```

### `next()`-এর চার রূপ

| ডাক | মানে | গল্প |
|---|---|---|
| `next()` | এই Handler শেষ, **পরের Handler-এ যাও** | "আমার কাজ শেষ, পরের কর্মী দেখুন" |
| `next("route")` | এই Route-এর **বাকি Handler বাদ**, **পরের মিলে-যাওয়া Route-এ যাও** | "এই সারিটা আমার জন্য না, বোর্ডের পরের সারি দেখুন" |
| `next("router")` | এই **Router থেকেই বেরিয়ে যাও** | "এই তলায় কাজ নেই, পরের তলা দেখুন" |
| `next(err)` | সোজা **Error Handler-এ** | "অভিযোগ ডেস্কে পাঠাও" |

`next("route")`-এর উদাহরণ:

```javascript
app.get("/dashboard", (req, res, next) => {
  if (req.get("x-role") !== "admin") return next("route");
  // admin না হলে এই Route-এর বাকি অংশ বাদ, নিচের পরের /dashboard Route-এ চলে যাও
  res.send("👑 Admin ড্যাশবোর্ড");
});

app.get("/dashboard", (req, res) => {
  res.send("🙂 সাধারণ ড্যাশবোর্ড");
});
```

### ক্রম — Route-এর সবচেয়ে গুরুত্বপূর্ণ নিয়ম

Express ওপর থেকে নিচে মেলায় এবং **প্রথম যেটা মিলে উত্তর দেয়, সেটাই জেতে**। তাই **নির্দিষ্ট (static) Route সবসময় সাধারণ (dynamic) Route-এর ওপরে** থাকবে।

```javascript
// ❌ ভুল ক্রম
app.get("/users/:id", (req, res) => res.json({ hit: "id", id: req.params.id }));
app.get("/users/me", (req, res) => res.json({ hit: "me" }));
// GET /users/me → { "hit": "id", "id": "me" }
// "me" কেই :id ধরে নেওয়া হলো! দ্বিতীয় Route কোনোদিনই চলবে না

// ✅ ঠিক ক্রম
app.get("/users/me", (req, res) => res.json({ hit: "me" }));
app.get("/users/:id", (req, res) => res.json({ hit: "id", id: req.params.id }));
```

আমাদের BookNest-এও ঠিক এই ঘটনা ঘটবে `GET /books/latest` আর `GET /books/:id` নিয়ে — `latest` অবশ্যই ওপরে।

```mermaid
flowchart TB
    subgraph BAD["❌ ভুল ক্রম"]
        direction TB
        B0["GET /books/latest এলো"] --> B1{"১. /books/:id<br/>মেলে?"}
        B1 -- "হ্যাঁ! id = latest" --> B2["💥 id-র জায়গায় 'latest' নিয়ে খুঁজতে গেল<br/>→ ভুল উত্তর বা error"]
    end
    subgraph GOOD["✅ ঠিক ক্রম"]
        direction TB
        G0["GET /books/latest এলো"] --> G1{"১. /books/latest<br/>মেলে?"}
        G1 -- "হ্যাঁ" --> G2["✅ সর্বশেষ বইয়ের Handler চললো"]
    end
```

**ক্রমের সাধারণ নিয়ম (পুরো অ্যাপের জন্য):**

| ক্রম | কী বসবে | কেন |
|---|---|---|
| ১ | সাধারণ Middleware (`cors`, logger, `express.json()`) | Route চলার **আগেই** body পড়া, log লেখা, অনুমতিপত্র যাচাই দরকার |
| ২ | নির্দিষ্ট (static) Route — `/books/latest` | আগে মিলিয়ে নিতে |
| ৩ | সাধারণ (dynamic) Route — `/books/:id` | নির্দিষ্টগুলো না মিললে তবেই এখানে |
| ৪ | **৪০৪ Handler** — `app.use(notFound)` | কোনো Route না মিললে |
| ৫ | **Error Handler** — `app.use((err, req, res, next) => ...)` | সবার শেষে, সব error ধরতে |

### "উত্তরও নেই, `next()`-ও নেই" bug

```javascript
app.get("/hang", (req, res) => {
  console.log("কাজ করলাম");
  // ❌ res.xxx() নেই, next() নেইও — ব্রাউজার ঘুরতেই থাকবে
});
```

**সমাধান:** কোড যত পথেই যাক — প্রতিটা পথের শেষে হয় `res.xxx()`, নয় `next()`, নয় error নিশ্চিত করুন। `if` এর ভেতরে `return` ভুলে গেলে উল্টো সমস্যা: উত্তর **দুবার** যায়।

---

## ৯. `app.use` বনাম `app.get`, `app.all` আর `app.route()`

### `app.use()` আর `app.get()` — একই মনে হলেও আলাদা

| | `app.use(path, fn)` | `app.get(path, fn)` |
|---|---|---|
| কোন Method | **সব** Method | শুধু GET |
| Path মেলানো | **শুরুর অংশ** মিললেই (prefix) | **পুরো path** হুবহু মিলতে হবে |
| Path থেকে prefix কাটে? | **হ্যাঁ** — ভেতরের `req.url` থেকে prefix বাদ যায় | না |
| সাধারণ ব্যবহার | Middleware বা Router বসানো | আসল Route |

```javascript
app.use("/prefix", (req, res, next) => {
  console.log(req.url);
  // /prefix/deep-এ request এলে এখানে req.url = "/deep" (prefix কেটে বাদ!)
  // পুরো ঠিকানা লাগলে req.originalUrl = "/prefix/deep"
  next();
});

app.use(logger);
// path না দিলে = "/" prefix = প্রতিটা request-এর জন্য চলবে। Application Middleware এভাবেই বসে
```

> এই "prefix কেটে ফেলা" ধর্মটাই **Router** কাজ করার ভিত্তি (পরের সেকশন)। আর এ কারণেই লগে `req.url` না লিখে `req.originalUrl` লিখতে হয়।

### `app.all()` — সব Method-এর জন্য একই Route

```javascript
app.all("/secret", (req, res) => {
  res.send(`এই ঠিকানায় যেকোনো Method চলে। আপনি এসেছেন ${req.method} নিয়ে`);
  // GET, POST, DELETE — সব Method-এই এই Handler চলবে
});
```

সাধারণত `app.all("/api/*splat", requireApiKey)` ধরনের "এই ঠিকানার নিচে সবকিছুর আগে একটা যাচাই" কাজে লাগে।

### `app.route()` — একই ঠিকানার সব Method একসাথে

`/books` ঠিকানাটা যদি GET আর POST দুটোর জন্য লাগে, তাহলে ঠিকানা দুবার লিখতে হয় — আর টাইপো হওয়ার ভয় থাকে। `route()` দিয়ে একবার লিখে পরপর Method জুড়ে দেওয়া যায়:

```javascript
// আগের লেখা: ঠিকানা বারবার
app.get("/chain", getHandler);
app.post("/chain", postHandler);

// ✅ route() দিয়ে: ঠিকানা একবার
app.route("/chain")
  .get((req, res) => res.send("GET chain"))
  .post((req, res) => res.send("POST chain"));
// PUT /chain দিলে কোনো Route মিলবে না → 404
```

Router-এও একই জিনিস: `router.route("/:id").get(...).patch(...).delete(...)`। আমাদের BookNest প্রজেক্টে এভাবেই লেখা হবে।

---

## ১০. `express.Router()` — তলা ম্যানেজার

### গল্প

মল পাঁচ তলা। প্রধান ম্যানেজার একা যদি শত শত দোকানের বোর্ড নিজের কাছে রাখেন, তাহলে একটা দোকান খুঁজতেই দিন কেটে যাবে। তাই প্রতি তলায় একজন করে **তলা ম্যানেজার (Router)**। ৩ তলার ম্যানেজার শুধু ৩ তলার দোকানগুলোর Route রাখেন। প্রধান ম্যানেজার (`app`) শুধু বলে দেন: **"`/books` দিয়ে শুরু যা-ই আসুক, ৩ তলার ম্যানেজারকে দাও।"**

`express.Router()` আসলে একটা **মিনি-অ্যাপ** — তার নিজের Route আর Middleware থাকতে পারে, শুধু নিজে port-এ শুনতে পারে না; তাকে কোনো `app`-এ বসাতে হয়।

### তলা ম্যানেজার বানানো ও বসানো

**`routes/bookRoutes.js`** (৩ তলার ম্যানেজার):

```javascript
const express = require("express");
const router = express.Router();
// router = একটা মিনি-অ্যাপ। app.get()-এর মতোই router.get(), router.post() লেখা যায়

router.get("/", (req, res) => {
  res.send("সব বইয়ের তালিকা");
  // ⚠️ এখানে path "/" লেখা, "/books" নয়! prefix (/books) বসানো হবে মাউন্টের সময়
});

router.get("/:id", (req, res) => {
  res.send(`${req.params.id} নম্বর বই`);
});

module.exports = router;
// এই router-টাকে অন্য ফাইলে ব্যবহারের জন্য export
```

**`playground.js`** (প্রধান ম্যানেজার):

```javascript
const bookRoutes = require("./routes/bookRoutes");

app.use("/books", bookRoutes);
// "/books দিয়ে শুরু যা-ই আসুক, bookRoutes-এর হাতে দাও"
// এখন GET /books        → router.get("/")
//      GET /books/7      → router.get("/:id")
```

> **সোনালি নিয়ম:** Router-এর ভেতরে path লেখা হয় **prefix ছাড়া**। মাউন্টের জায়গায় prefix যা দেবেন (`/books`, `/api/v1/books`), ভেতরের Route-এ কোনো বদল লাগবে না। ফলে prefix বদলানো একটা লাইনের কাজ।

### `req.url`, `req.baseUrl`, `req.originalUrl` — Router-এর ভেতরে কে কী

`app.use("/api/v1/things", router)` করা আছে, আর request এলো `GET /api/v1/things/42?x=1`:

| Property | মান | মানে |
|---|---|---|
| `req.originalUrl` | `/api/v1/things/42?x=1` | ক্রেতা আসলে যা পাঠিয়েছে — **অপরিবর্তিত** |
| `req.baseUrl` | `/api/v1/things` | Router যে prefix-এ বসানো |
| `req.url` | `/42?x=1` | prefix বাদ দিয়ে Router-এর ভেতরে যা দেখা যায় |

তাই logger বা error বার্তায় **`req.originalUrl`** ব্যবহার করুন।

### মাউন্ট-করা Router-এর গাছ

Router-এর ভেতরে আরেকটা Router বসানো যায় — গাছের মতো ডালপালা ছড়ায়। আমাদের BookNest-এর চূড়ান্ত গাছ:

```mermaid
flowchart TB
    APP["🏬 app.js"] -- "app.use('/api/v1')" --> IDX["routes/index.js"]
    IDX -- "router.use('/auth')" --> AUTH["auth.routes.js"]
    IDX -- "router.use('/books')" --> BOOK["book.routes.js"]
    BOOK -- "router.use('/:bookId/reviews')" --> REV["review.routes.js"]
    AUTH --> A1["POST /register<br/>POST /login<br/>GET /me"]
    BOOK --> B1["GET / · POST /<br/>GET /latest<br/>GET · PUT · PATCH · DELETE /:id"]
    REV --> R1["GET / · POST /<br/>DELETE /:reviewId"]
```

### Router-Level Middleware — `router.use()`

শুধু এই Router-এর Route গুলোর জন্য একটা Middleware:

```javascript
router.use((req, res, next) => {
  console.log("📚 বইঘরে ঢুকছে:", req.method, req.originalUrl);
  next();
});
// এটা শুধু bookRoutes-এর Route গুলোর আগে চলবে। /auth বা অন্য Router-এর জন্য নয়
```

### `router.param()` — `:id` থাকলেই আগে যাচাই

একই `:id` যদি ৫টা Route-এ থাকে, ৫ জায়গায় "id ঠিক আছে কিনা" লিখতে হবে না। `router.param("id", fn)` বলে দেয়: **"এই Router-এর যেকোনো path-এ `:id` থাকলে আগে এই যাচাই চালাও।"**

```javascript
router.param("id", (req, res, next, value) => {
  // পাঁচটা parameter: (req, res, next, value, name)
  // value = URL থেকে আসা মান, যেমন "abc"। name = parameter-এর নাম, এখানে "id"

  if (!/^\d+$/.test(value)) {
    return res.status(400).json({ error: `"${value}" একটা সঠিক id নয়।` });
    // /^\d+$/ = শুরু থেকে শেষ পর্যন্ত শুধু সংখ্যা। Express 5-এ path-এ regex দিতে পারি না, তাই যাচাই এখানে
  }

  next();
  // ঠিক থাকলে পরের ধাপে (আসল Route Handler)
});

router.get("/:id", (req, res) => res.json({ id: Number(req.params.id) }));
// GET /books/12  → { "id": 12 }
// GET /books/abc → 400 { "error": "\"abc\" একটা সঠিক id নয়।" }
```

> `router.param()` শুধু ওই Router-এর নিজের path-এর parameter-এর জন্য চলে (এবং `router.use("/:bookId/reviews", ...)`-এর মতো mount path-এর জন্যও)। আমাদের আসল প্রজেক্টে এটা দিয়ে MongoDB-র `_id`-র ফরম্যাট যাচাই করবো।

### `mergeParams` — নিচের Router ওপরের Parameter দেখতে পায় না!

Nested Route-এর সবচেয়ে বিখ্যাত ফাঁদ। ধরুন:

```javascript
const reviewRouter = express.Router();
reviewRouter.get("/", (req, res) => res.json({ bookId: req.params.bookId }));

app.use("/books/:bookId/reviews", reviewRouter);
// GET /books/B1/reviews → { }   ← bookId হারিয়ে গেল, req.params.bookId = undefined!
```

কারণ: **ডিফল্টে প্রতিটা Router শুধু নিজের path-এর parameter দেখে**। ওপরের (parent) path-এ `:bookId` আছে, কিন্তু `reviewRouter`-এর নিজের path-এ নেই। সমাধান:

```javascript
const reviewRouter = express.Router({ mergeParams: true });
// mergeParams: true = "parent-এর parameter গুলোও আমাকে দেখতে দাও"

reviewRouter.get("/", (req, res) => res.json({ bookId: req.params.bookId }));
// GET /books/B1/reviews → { "bookId": "B1" } ✅
```

### Router-এর অপশন

| অপশন | ডিফল্ট | কাজ |
|---|---|---|
| `mergeParams` | `false` | parent path-এর parameter দেখা যাবে কিনা |
| `caseSensitive` | `false` | `/Books` আর `/books` আলাদা ধরা হবে কিনা |
| `strict` | `false` | শেষের `/` (`/books/`) আলাদা ধরা হবে কিনা |

### একই Router দুই জায়গায়

```javascript
app.use("/api/v1/books", bookRoutes);
app.use("/api/v2/books", bookRoutes);
// একই Router দুই prefix-এ। API-র নতুন version আনার সময় কাজে লাগে
```

---

## ১১. Controller — কর্মীর কাজ আলাদা ফাইলে

### গল্প

ডিরেক্টরি বোর্ডে প্রতিটা সারির শেষে লেখা থাকে শুধু **কর্মীর নাম** — "বিক্রয়কর্মী ৩"। ৩ নম্বর কর্মী ঠিক কীভাবে কাজ করেন (কোন খাতা দেখেন, কোন তাকে যান, কী কী যাচাই করেন), সেই দশ পাতার বিবরণ যদি **বোর্ডেই** লিখে দেওয়া হতো, বোর্ড পড়া অসম্ভব হয়ে যেতো। তাই বোর্ডে শুধু নাম, আর কাজের বিবরণ থাকে কর্মীর নিজস্ব ফাইলে।

Express-এও একই ব্যাপার। **Route ফাইল** = বোর্ড ("কোথায় কী, কে করবে")। **Controller ফাইল** = কর্মীর কাজের বিবরণ ("কীভাবে করবে")।

### আগে ও পরে

```javascript
// ❌ Route-এর ভেতরেই সব কাজ (বোর্ডে দশ পাতার বিবরণ)
router.get("/:id", async (req, res) => {
  const book = await Book.findById(req.params.id);
  if (!book) {
    return res.status(404).json({ success: false, message: "বই পাওয়া যায়নি" });
  }
  res.json({ success: true, data: book });
});
router.post("/", protect, async (req, res) => {
  // আরও ২০ লাইন ...
});
// ৫টা Route হলে ফাইল ১০০ লাইনের বেশি, কোন ঠিকানায় কী আছে এক নজরে বোঝা অসম্ভব
```

```javascript
// ✅ Route ফাইল — শুধু বোর্ড (এক নজরে পুরো নকশা)
router.get("/", controller.getBooks);
router.post("/", protect, controller.createBook);
router.get("/:id", controller.getBook);
```

```javascript
// ✅ Controller ফাইল — কর্মীর কাজের বিবরণ
const getBook = async (req, res) => {
  const book = await findBookOrFail(req.params.id);
  res.json({ success: true, data: book });
};

module.exports = { getBooks, createBook, getBook };
```

### সম্পর্কটা এক ছবিতে

```mermaid
flowchart LR
    RQ["📨 Request"] --> RT["📋 Route<br/>কোথায়? কোন Method?<br/>কোন Middleware?"]
    RT --> MW["🚧 Middleware<br/>protect, restrictTo"]
    MW --> CT["🧑‍💼 Controller<br/>কী করতে হবে?<br/>req পড়ো, যাচাই করো,<br/>উত্তর বানাও"]
    CT --> MD["📦 Model<br/>ডেটা কেমন?<br/>DB-র সাথে কথা"]
    MD --> DB[("🗄️ MongoDB")]
```

| ফাইল | দায়িত্ব | `req`/`res` চেনে? |
|---|---|---|
| **Route** (`routes/`) | ঠিকানা + Method + কোন Middleware + কোন Controller | — |
| **Controller** (`controllers/`) | `req` থেকে তথ্য নেওয়া, যাচাই, Model ডাকা, `res` দিয়ে উত্তর | ✅ হ্যাঁ |
| **Model** (`models/`) | ডেটার গঠন, নিয়ম, DB-র সাথে যোগাযোগ | ❌ না |

> এটাকেই লোকে **MVC**-র মতো কাঠামো বলে (Model – View – Controller)। API-তে "View" হলো JSON উত্তর (বা React)। বড় প্রজেক্টে Controller আর Model-এর মাঝে আলাদা একটা **Service** স্তরও রাখা হয়, কিন্তু আমাদের ছোট প্রজেক্টে এই তিনটাই যথেষ্ট।

### সবচেয়ে সাধারণ ভুল — ফাংশন **ডেকে** ফেলা

```javascript
router.get("/", controller.getBooks());   // ❌ () দিয়ে ফাংশনটা এখনই চালিয়ে ফেলা হলো!
router.get("/", controller.getBooks);     // ✅ শুধু ফাংশনের নামটা দেওয়া হলো, Request এলে Express নিজে চালাবে
```

> কর্মীর **নাম** বোর্ডে লিখতে হয়, কর্মীকে ধরে এনে কাজে **লাগিয়ে** বোর্ডে বসিয়ে দিলে চলে না।

---

## ১২. Middleware দিয়ে Route পাহারা — Auth ও Role

### গল্প

মলের গেট থেকে বইঘর পর্যন্ত যাওয়ার পথে কয়েকটা **চেকপোস্ট**:

1. **লগ খাতায় নাম লেখা** — কে কখন ঢুকলো (`logger`)
2. **ব্যাগ/পার্সেল খোলা** — ক্রেতার আনা JSON পড়া (`express.json()`)
3. **সদস্য কার্ড দেখা** — লগইন করা কি না (`protect`)
4. **VIP লাউঞ্জের গার্ড** — পদমর্যাদা দেখা, শুধু admin ঢুকতে পারবে (`restrictTo("admin")`)

এই প্রতিটা চেকপোস্টই একটা **Middleware** — `(req, res, next)` তিন parameter-ওয়ালা ফাংশন, যেটা Request আর আসল Handler-এর **মাঝখানে** বসে। প্রতিটা চেকপোস্ট চারটার একটা করতে পারে:

- নিজের কাজ (যেমন লগ লেখা) করে **`next()`** ডাকতে পারে;
- `req`/`res`-এ কিছু **জুড়ে দিতে** পারে (যেমন `req.user = ...`);
- ক্রেতাকে **সরাসরি ফিরিয়ে দিতে** পারে (`res.status(401)...`) — তখন পরের ধাপে আর যাওয়া হয় না;
- সমস্যা হলে **error ছুঁড়ে** দিতে পারে।

### Middleware কোথায় বসে — তিন স্তর

| স্তর | কীভাবে | কার জন্য | উদাহরণ |
|---|---|---|---|
| **Application** | `app.use(mw)` | পুরো অ্যাপের **সব** Request | `cors`, logger, `express.json()` |
| **Router** | `router.use(mw)` | ওই Router-এর **সব** Route | শুধু `/books`-এর জন্য একটা log |
| **Route** | `router.post("/", mw, handler)` | **শুধু ওই একটা** Route | `protect`, `restrictTo("admin")` |

```javascript
app.use(logger);                                     // ১. Application — সবার জন্য
router.use(bookLogger);                              // ২. Router — শুধু books-এর জন্য
router.delete("/:id", protect, restrictTo("admin"), deleteBook);   // ৩. Route — শুধু এই একটার জন্য
```

### তিন জাতের Middleware

| জাত | উদাহরণ |
|---|---|
| **Built-in** (Express-এর সাথেই আসে) | `express.json()`, `express.urlencoded()`, `express.static()` |
| **Third-party** (npm থেকে) | `cors`, `morgan` (logger), `helmet` (নিরাপত্তা header), `express-rate-limit`, `multer` |
| **Custom** (নিজে বানানো) | আমাদের `logger`, `protect`, `restrictTo`, `validateObjectId` |

### Custom Middleware ১: `logger`

**`src/middleware/logger.js`**

```javascript
// logger = প্রতিটা request-এর হিসাব খাতায় লেখে: কোন method, কোন ঠিকানা, কী উত্তর গেল, কত ms লাগলো
const logger = (req, res, next) => {
  const start = Date.now();
  // start = request ঢোকার মুহূর্ত (মিলিসেকেন্ডে)

  res.on("finish", () => {
    // "finish" event = উত্তর পুরোপুরি পাঠানো হয়ে গেলে এই ফাংশন চলে
    console.log(`${req.method} ${req.originalUrl} → ${res.statusCode} (${Date.now() - start}ms)`);
    // req.originalUrl = Router-এর prefix সহ পুরো ঠিকানা (req.url নয়, কারণ Router ভেতরে prefix কেটে ফেলে)
  });

  next();
  // next() = "আমার কাজ শেষ, পরের জনের কাছে যাও"
};

module.exports = logger;
```

### JWT — সদস্য কার্ডের গল্প

ব্যবহারকারী লগইন করলে সার্ভার তাকে একটা **সদস্য কার্ড (JWT token)** ছাপিয়ে দেয়। এরপর যতবার সে কোনো সুরক্ষিত Route-এ যায়, কার্ডটা দেখাতে হয় (`Authorization: Bearer <token>` header-এ)। গার্ড (`protect`) কার্ডের **সিল-ছাপ (signature)** যাচাই করে দেখে কার্ডটা আসল কিনা আর মেয়াদ আছে কিনা।

```
eyJhbGciOiJIUzI1NiJ9 . eyJpZCI6IjY0ZjEuLi4ifQ . 3kX9...
└──── Header ────┘     └──── Payload ────┘      └─ Signature ─┘
  (কোন অ্যালগরিদম)    (আমাদের ঢোকানো তথ্য,       (গোপন চাবি দিয়ে সিল —
                        যেমন user-এর id)           কেউ বদলালে ধরা পড়ে)
```

> ⚠️ **Payload এনক্রিপ্টেড নয়, শুধু সিল করা** — যে কেউ base64 decode করে পড়তে পারে। তাই token-এ password বা গোপন কিছু **কখনো** রাখবেন না; শুধু `id` (আর দরকার হলে `role`) রাখুন।

```mermaid
sequenceDiagram
    autonumber
    participant U as 👤 তিথি (React)
    participant S as 🛎️ Express
    participant DB as 🗄️ MongoDB
    U->>S: POST /auth/login { email, password }
    S->>DB: email দিয়ে user খোঁজো
    DB-->>S: user + password hash
    S->>S: bcrypt দিয়ে password মেলানো
    S-->>U: 200 OK + token (সদস্য কার্ড)
    Note over U: token সংরক্ষণ করে রাখলো
    U->>S: POST /books + Authorization: Bearer token
    S->>S: protect: token যাচাই, req.user বসানো
    S->>DB: নতুন বই সংরক্ষণ
    S-->>U: 201 Created
```

### Custom Middleware ২: `protect` আর `restrictTo`

**`src/middleware/auth.js`**

```javascript
const jwt = require("jsonwebtoken");
const User = require("../models/User");
const ApiError = require("../utils/ApiError");

// protect = "সদস্য কার্ড আছে কি?" — লগইন করা ছাড়া যে route-এ ঢোকা নিষেধ, সেখানে বসে
const protect = async (req, res, next) => {
  const authHeader = req.get("authorization");
  // authHeader = "Bearer eyJhbGci..." ধরনের পুরো লাইন। না দিলে undefined

  if (!authHeader || !authHeader.startsWith("Bearer ")) {
    throw new ApiError(401, "লগইন করা নেই। Authorization: Bearer <token> header পাঠান।");
  }

  const token = authHeader.split(" ")[1];
  // token = "Bearer " এর পরের অংশ

  const decoded = jwt.verify(token, process.env.JWT_SECRET);
  // jwt.verify() = token আসল কিনা আর মেয়াদ আছে কিনা যাচাই করে। ভুল হলে নিজেই error throw করে
  // (JsonWebTokenError বা TokenExpiredError) — সেটা Error Handler ধরবে
  // decoded = আমরা token বানানোর সময় যা ঢুকিয়েছিলাম, যেমন { id: "...", iat: ..., exp: ... }

  const user = await User.findById(decoded.id);
  if (!user) {
    throw new ApiError(401, "এই token-এর ব্যবহারকারী আর নেই।");
  }

  req.user = user;
  // req.user = পরের সব handler-এর জন্য "কে এই request পাঠিয়েছে" — req object-এ নিজের জিনিস ঝুলিয়ে দেওয়া যায়
  next();
};

// restrictTo = "এই ঘরে শুধু অমুক পদের লোক" — role দেখে ঢুকতে দেয়
const restrictTo = (...roles) => {
  // ...roles = rest parameter। restrictTo("admin", "editor") দিলে roles = ["admin", "editor"]
  return (req, res, next) => {
    // এই ভেতরের ফাংশনটাই আসল middleware। বাইরের ফাংশন শুধু roles মনে রাখার জন্য (closure)
    if (!roles.includes(req.user.role)) {
      throw new ApiError(403, "এই কাজের অনুমতি আপনার নেই।");
    }
    next();
  };
};

module.exports = { protect, restrictTo };
```

**এখানে কয়েকটা জিনিস খেয়াল করুন:**

| বিষয় | ব্যাখ্যা |
|---|---|
| `protect` **নিজেই** একটা Middleware | তাই Route-এ লেখা হয় `protect` (ডাকা হয় না) |
| `restrictTo` একটা **ফাংশন-বানানো ফাংশন** | `restrictTo("admin")` চালালে সে একটা Middleware **ফেরত দেয়** — তাই `()` দিয়ে ডাকতে হয় |
| `restrictTo`-এর ভেতরের ফাংশন `roles` মনে রাখে | একে বলে **closure**। এভাবেই একই কোড দিয়ে `restrictTo("admin")` আর `restrictTo("admin", "editor")` দুটোই চলে |
| `req.user = user` | পরের সব Handler-এ জানা যাবে "কে পাঠিয়েছে" — `req`-এ নিজের জিনিস জুড়ে দেওয়া Middleware-এর একটা প্রধান কাজ |
| `protect` আগে, `restrictTo` পরে | `restrictTo` `req.user.role` পড়ে — `req.user` না থাকলে ভেঙে যাবে। ক্রমটাই গুরুত্বপূর্ণ |
| `throw new ApiError(401, ...)` | সরাসরি `res` দিয়ে উত্তর না দিয়ে error ছুঁড়ে দিলাম — Error Handler (১৭ নম্বর সেকশন) সুন্দরভাবে সামলাবে |

### Custom Middleware ৩: `validateObjectId`

MongoDB-র `_id` হলো ২৪ অক্ষরের hex String। কেউ `/books/12345` দিলে Mongoose একটা কুৎসিত `CastError` ছুঁড়তো। আমরা আগে থেকেই ঠেকিয়ে দিই, আর `router.param()` দিয়ে (১০ নম্বর সেকশনে যেমন দেখলাম) এটা প্রতিটা `:id`-র আগে নিজে থেকে চলবে।

**`src/middleware/validateObjectId.js`**

```javascript
const ApiError = require("../utils/ApiError");

const OBJECT_ID_PATTERN = /^[0-9a-fA-F]{24}$/;
// MongoDB-র _id মানে ২৪ অক্ষরের hex (0-9, a-f) String

const validateObjectId = (req, res, next, value, name) => {
  // এটা router.param()-এর জন্য বানানো — তাই parameter পাঁচটা: (req, res, next, value, name)
  // value = URL থেকে আসা মান (যেমন "64f1..."), name = parameter-এর নাম (যেমন "id")
  if (!OBJECT_ID_PATTERN.test(value)) {
    throw new ApiError(400, `"${value}" একটা সঠিক ${name} নয়।`);
  }
  next();
};

module.exports = validateObjectId;
```

### Route-এ Middleware সাজালে একটা Request-এর যাত্রা

```javascript
router.delete("/:id", protect, restrictTo("admin"), controller.deleteBook);
```

```mermaid
sequenceDiagram
    autonumber
    participant C as 👤 Client
    participant V as param id যাচাই
    participant P as protect
    participant R as restrictTo admin
    participant H as deleteBook Controller
    C->>V: DELETE /api/v1/books/7 + Bearer token
    alt id-র ফরম্যাট ভুল
        V-->>C: 400 Bad Request
    else id ঠিক
        V->>P: next()
        alt token নেই বা ভুল
            P-->>C: 401 Unauthorized
        else token ঠিক
            P->>P: user খুঁজে req.user বসালো
            P->>R: next()
            alt user admin নয়
                R-->>C: 403 Forbidden
            else user admin
                R->>H: next()
                H->>H: বই খুঁজে মুছলো (না পেলে 404)
                H-->>C: 204 No Content
            end
        end
    end
```

### বাড়তি Middleware — প্রায় প্রতিটা প্রজেক্টে যা লাগে

| প্যাকেজ | কাজ | ব্যবহার |
|---|---|---|
| `helmet` | নিরাপত্তার জন্য দরকারি HTTP header বসায় | `app.use(helmet())` |
| `morgan` | তৈরি করা logger (আমাদেরটার বদলে) | `app.use(morgan("dev"))` |
| `express-rate-limit` | এক IP থেকে কত বার Request আসতে পারবে তার সীমা (login-এ brute force ঠেকাতে) | Route-এ বা `app.use` |
| `compression` | উত্তর ছোট (gzip) করে পাঠায় | `app.use(compression())` |

---

## ১৩. পুরো প্রজেক্ট: BookNest API-র কাঠামো ও Model

### গল্প

খেলার মাঠ পার হয়ে এবার আসল মল বানানোর পালা — এবার গুদাম (MongoDB) থাকবে, ক্রেতা সদস্য হতে পারবে, লগইন করতে পারবে, আর প্রতিটা তলার নিজস্ব ম্যানেজার থাকবে। নতুন করে সাজাতে প্রজেক্ট ফোল্ডার থেকে `playground.js` সরিয়ে রাখুন (বা আলাদা ফোল্ডারে নতুন প্রজেক্ট শুরু করুন)। আগের `npm install` করা প্যাকেজগুলোই লাগবে।

### ফোল্ডার স্ট্রাকচার

```
📦 booknest-api/
 ┣ 📂 src/
 ┃ ┣ 📂 config/
 ┃ ┃ ┗ 📜 db.js                    ← MongoDB সংযোগ
 ┃ ┣ 📂 models/                    ← ডেটার গঠন (গুদামের তাক)
 ┃ ┃ ┣ 📜 User.js
 ┃ ┃ ┣ 📜 Book.js
 ┃ ┃ ┗ 📜 Review.js
 ┃ ┣ 📂 routes/                    ← ডিরেক্টরি বোর্ড (তলা ম্যানেজার)
 ┃ ┃ ┣ 📜 index.js                 ← সব Router-এর জোড়ার জায়গা
 ┃ ┃ ┣ 📜 auth.routes.js
 ┃ ┃ ┣ 📜 book.routes.js
 ┃ ┃ ┗ 📜 review.routes.js
 ┃ ┣ 📂 controllers/               ← বিক্রয়কর্মী (আসল কাজ)
 ┃ ┃ ┣ 📜 auth.controller.js
 ┃ ┃ ┣ 📜 book.controller.js
 ┃ ┃ ┗ 📜 review.controller.js
 ┃ ┣ 📂 middleware/                ← চেকপোস্ট
 ┃ ┃ ┣ 📜 auth.js                  ← protect, restrictTo
 ┃ ┃ ┣ 📜 logger.js
 ┃ ┃ ┣ 📜 validateObjectId.js
 ┃ ┃ ┣ 📜 notFound.js
 ┃ ┃ ┗ 📜 errorHandler.js
 ┃ ┣ 📂 utils/
 ┃ ┃ ┗ 📜 ApiError.js
 ┃ ┗ 📜 app.js                     ← Express অ্যাপ সাজানো (listen নেই)
 ┣ 📜 server.js                    ← DB সংযোগ + app.listen
 ┣ 📜 .env                         ← গোপন চাবি (GitHub-এ যাবে না!)
 ┣ 📜 .gitignore
 ┗ 📜 package.json
```

> **`app.js` আর `server.js` আলাদা কেন?** `app.js` শুধু অ্যাপ বানায় ও সাজায়, port খোলে না। `server.js` DB জোড়া লাগায় আর port খোলে। এতে ভবিষ্যতে টেস্ট লিখলে (`supertest`) DB বা port ছাড়াই `app` টেস্ট করা যায়।

### কোন ফাইল কোন সেকশনে

| ফাইল | সেকশন |
|---|---|
| `middleware/logger.js`, `auth.js`, `validateObjectId.js` | ১২ (আগেই দেখেছি) |
| `utils/ApiError.js`, `config/db.js`, তিনটা `models/` | ১৩ (এখানে) |
| `book.routes.js` + `book.controller.js` | ১৪ |
| `auth.routes.js` + `auth.controller.js` | ১৫ |
| `review.routes.js` + `review.controller.js` | ১৬ |
| `notFound.js`, `errorHandler.js` | ১৭ |
| `routes/index.js`, `app.js`, `server.js` | ১৮ |

### `.env` আর `.gitignore`

**`.env`**

```bash
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://127.0.0.1:27017/booknest
JWT_SECRET=এখানে_খুব_লম্বা_আর_এলোমেলো_একটা_লাইন_বসান
JWT_EXPIRES_IN=7d
CLIENT_URL=http://localhost:5173
```

| চাবি | কাজ |
|---|---|
| `MONGO_URI` | MongoDB-র ঠিকানা। লোকাল হলে ওপরেরটা; Atlas হলে `mongodb+srv://<user>:<password>@<cluster>.mongodb.net/booknest` |
| `JWT_SECRET` | সদস্য কার্ডে সিল দেওয়ার গোপন চাবি। ফাঁস হলে যে কেউ নকল কার্ড বানাতে পারবে |
| `JWT_EXPIRES_IN` | কার্ডের মেয়াদ (`7d` = ৭ দিন) |
| `CLIENT_URL` | React অ্যাপ কোন ঠিকানায় চলে (CORS-এর জন্য, ১৯ নম্বর সেকশন) |

**`.gitignore`**

```bash
node_modules/     # npm install দিলেই ফিরে আসে
.env              # গোপন চাবি — কখনোই GitHub-এ নয়
```

### সব Endpoint-এর তালিকা — আমাদের Route Table

এটাই BookNest-এর পুরো "ডিরেক্টরি বোর্ড"। পরের সেকশনগুলোতে এই সারিগুলো একে একে বানাবো।

| # | Method | URL | কে ঢুকতে পারবে | সফল | সম্ভাব্য ব্যর্থতা |
|---|---|---|---|---|---|
| 1 | GET | `/api/v1/health` | সবাই | 200 | — |
| 2 | POST | `/api/v1/auth/register` | সবাই | 201 | 400, 409 |
| 3 | POST | `/api/v1/auth/login` | সবাই | 200 | 400, 401 |
| 4 | GET | `/api/v1/auth/me` | লগইন করা | 200 | 401 |
| 5 | GET | `/api/v1/books` | সবাই | 200 | 400 |
| 6 | GET | `/api/v1/books/latest` | সবাই | 200 | — |
| 7 | GET | `/api/v1/books/:id` | সবাই | 200 | 400, 404 |
| 8 | POST | `/api/v1/books` | লগইন করা | 201 | 400, 401 |
| 9 | PUT | `/api/v1/books/:id` | বইয়ের মালিক বা admin | 200 | 400, 401, 403, 404 |
| 10 | PATCH | `/api/v1/books/:id` | বইয়ের মালিক বা admin | 200 | 400, 401, 403, 404 |
| 11 | DELETE | `/api/v1/books/:id` | **শুধু admin** | 204 | 400, 401, 403, 404 |
| 12 | GET | `/api/v1/books/:bookId/reviews` | সবাই | 200 | 400 |
| 13 | POST | `/api/v1/books/:bookId/reviews` | লগইন করা | 201 | 400, 401, 404, 409 |
| 14 | DELETE | `/api/v1/books/:bookId/reviews/:reviewId` | রিভিউয়ের মালিক বা admin | 204 | 400, 401, 403, 404 |

### সাহায্যকারী ফাইল দুটো

**`src/utils/ApiError.js`** — নিজের বানানো "ইচ্ছাকৃত ভুল"

```javascript
// ApiError = আমাদের নিজের বানানো "ইচ্ছাকৃত ভুল"। যেমন: বই পাওয়া যায়নি (404), অনুমতি নেই (403)
class ApiError extends Error {
  constructor(statusCode, message) {
    super(message);
    // super(message) = সাধারণ Error-এর মতোই message বসায়
    this.statusCode = statusCode;
    // statusCode = কোন HTTP status পাঠাতে হবে (404, 403, 400...) — Error Handler এটা পড়ে উত্তর বানাবে
  }
}

module.exports = ApiError;
```

**`src/config/db.js`**

```javascript
const mongoose = require("mongoose");

const connectDB = async () => {
  await mongoose.connect(process.env.MONGO_URI);
  // mongoose.connect() = MongoDB-র সাথে সংযোগ। সংযোগ না হলে error throw করে — সেটা server.js-এ ধরবো
  console.log("✅ MongoDB সংযুক্ত হয়েছে");
};

module.exports = connectDB;
```

### Model তিনটা — গুদামের তাক

Model-এর ভেতরের কাজ আজকের বিষয় নয়, তবে Route-এর যুক্তি বুঝতে এদের গঠন জানা দরকার।

**`src/models/User.js`** — সদস্য

```javascript
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

const userSchema = new mongoose.Schema(
  {
    name: { type: String, required: [true, "নাম দিতে হবে"], trim: true },
    email: { type: String, required: [true, "ইমেইল দিতে হবে"], unique: true, lowercase: true, trim: true },
    password: { type: String, required: [true, "password দিতে হবে"], minlength: [6, "password কমপক্ষে ৬ অক্ষরের হতে হবে"], select: false },
    // select: false = কোনো query-তে ডিফল্টে password ফেরত আসবে না (লগইনের সময় .select("+password") দিয়ে চেয়ে নিতে হয়)
    role: { type: String, enum: ["user", "admin"], default: "user" },
  },
  { timestamps: true }
);

userSchema.pre("save", async function () {
  // save করার ঠিক আগে এই ফাংশন চলে। এখানে arrow function নয়, সাধারণ function — কারণ this দিয়ে ডকুমেন্টটা ধরতে হবে
  if (!this.isModified("password")) return;
  // password বদলায়নি (শুধু নাম বদলেছে) হলে আবার hash করার দরকার নেই
  this.password = await bcrypt.hash(this.password, 10);
  // 10 = salt round। সংখ্যা যত বড়, hash তত ধীর ও নিরাপদ
});

userSchema.methods.comparePassword = function (plainPassword) {
  return bcrypt.compare(plainPassword, this.password);
  // ব্যবহারকারীর দেওয়া password আর DB-র hash মেলানো — true/false ফেরত দেয়
};

module.exports = mongoose.model("User", userSchema);
```

**`src/models/Book.js`** — বই

```javascript
const mongoose = require("mongoose");

const bookSchema = new mongoose.Schema(
  {
    title: { type: String, required: [true, "বইয়ের নাম দিতে হবে"], trim: true },
    author: { type: String, required: [true, "লেখকের নাম দিতে হবে"], trim: true },
    price: { type: Number, required: [true, "দাম দিতে হবে"], min: [0, "দাম ঋণাত্মক হতে পারে না"] },
    category: {
      type: String,
      enum: { values: ["novel", "poetry", "science", "history", "kids"], message: "category অবশ্যই novel, poetry, science, history বা kids হতে হবে" },
      default: "novel",
    },
    inStock: { type: Boolean, default: true },
    createdBy: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true },
    // createdBy = কোন ব্যবহারকারী বইটা যোগ করেছে (User collection-এর _id)
  },
  { timestamps: true }
  // timestamps: true = createdAt আর updatedAt নিজে থেকে বসবে
);

module.exports = mongoose.model("Book", bookSchema);
```

**`src/models/Review.js`** — রিভিউ

```javascript
const mongoose = require("mongoose");

const reviewSchema = new mongoose.Schema(
  {
    book: { type: mongoose.Schema.Types.ObjectId, ref: "Book", required: true },
    user: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true },
    rating: { type: Number, required: [true, "রেটিং দিতে হবে"], min: [1, "রেটিং ১ থেকে ৫-এর মধ্যে হবে"], max: [5, "রেটিং ১ থেকে ৫-এর মধ্যে হবে"] },
    comment: { type: String, trim: true, maxlength: 500 },
  },
  { timestamps: true }
);

reviewSchema.index({ book: 1, user: 1 }, { unique: true });
// একজন ব্যবহারকারী একটা বইয়ে একটাই রিভিউ দিতে পারবে — দ্বিতীয়বার দিলে duplicate key error (11000) আসবে

module.exports = mongoose.model("Review", reviewSchema);
```

```mermaid
erDiagram
    USER ||--o{ BOOK : "যোগ করে (createdBy)"
    USER ||--o{ REVIEW : "লেখে (user)"
    BOOK ||--o{ REVIEW : "পায় (book)"
    USER {
        ObjectId _id
        string name
        string email
        string password
        string role
    }
    BOOK {
        ObjectId _id
        string title
        string author
        number price
        string category
        boolean inStock
    }
    REVIEW {
        ObjectId _id
        number rating
        string comment
    }
```

---

## ১৪. Books Route ও Controller

### গল্প

এবার ৩ তলার **বইঘর**। এখানকার তলা ম্যানেজার (`book.routes.js`) নিজের বোর্ড সাজিয়ে রাখেন, আর প্রতিটা সারির জন্য একজন করে কর্মী (`book.controller.js`) আছেন।

### Route ফাইল — বইঘরের বোর্ড

**`src/routes/book.routes.js`**

```javascript
const express = require("express");
const controller = require("../controllers/book.controller");
const { protect, restrictTo } = require("../middleware/auth");
const validateObjectId = require("../middleware/validateObjectId");
const reviewRoutes = require("./review.routes");

const router = express.Router();

router.param("id", validateObjectId);
router.param("bookId", validateObjectId);
// router.param = কোনো path-এ :id বা :bookId থাকলেই তার আগে এই যাচাই নিজে থেকে চলবে। প্রতিটা route-এ আলাদা করে লিখতে হচ্ছে না

router.use("/:bookId/reviews", reviewRoutes);
// Nested Router = /books/৭/reviews দিয়ে শুরু হওয়া সব request review-এর Router-এর হাতে ছেড়ে দেওয়া

router.get("/latest", controller.getLatestBooks);
// ⚠️ স্ট্যাটিক path "/latest" অবশ্যই "/:id"-এর আগে। উল্টো হলে "latest"-কেই id ধরে নেওয়া হতো

router.route("/").get(controller.getBooks).post(protect, controller.createBook);

router
  .route("/:id")
  .get(controller.getBook)
  .put(protect, controller.replaceBook)
  .patch(protect, controller.updateBook)
  .delete(protect, restrictTo("admin"), controller.deleteBook);

module.exports = router;
```

**এই ছোট ফাইলে এতগুলো ধারণা একসাথে — একটা একটা করে পড়ুন:**

| লাইন | ধারণা | কোন সেকশনে শিখেছি |
|---|---|---|
| `express.Router()` | তলা ম্যানেজার বানানো | ১০ |
| `router.param("id", validateObjectId)` | যেকোনো `:id`-র আগে ফরম্যাট যাচাই | ১০ |
| `router.use("/:bookId/reviews", reviewRoutes)` | Nested Router বসানো | ১০, ১৬ |
| `router.get("/latest", ...)` **আগে**, `/:id` **পরে** | Route-এর ক্রম | ৮ |
| `router.route("/").get(...).post(...)` | একই ঠিকানার একাধিক Method | ৯ |
| `.post(protect, controller.createBook)` | Route Middleware | ১২ |
| `.delete(protect, restrictTo("admin"), ...)` | Middleware-এর সারি | ১২ |
| `controller.getBooks` (ডাকা নেই!) | ফাংশনের নাম দেওয়া | ১১ |

### Controller ফাইল — বইঘরের কর্মীরা

**`src/controllers/book.controller.js`**

```javascript
const Book = require("../models/Book");
const ApiError = require("../utils/ApiError");

// ---------- ছোট সহায়ক ফাংশন ----------

const escapeRegex = (text) => text.replace(/[.*+?^${}()|[\]\\]/g, "\\$&");
// ব্যবহারকারীর সার্চ লেখায় . * ( ) এই বিশেষ চিহ্ন থাকলে regex ভেঙে যায় (বা ReDoS হয়) — তাই আগে "\" দিয়ে সাধারণ অক্ষর বানিয়ে নিলাম

const toNumberOrFail = (value, fieldName) => {
  const number = Number(value);
  if (Number.isNaN(number)) {
    throw new ApiError(400, `${fieldName} অবশ্যই একটা সংখ্যা হতে হবে।`);
  }
  return number;
};

const findBookOrFail = async (id) => {
  const book = await Book.findById(id);
  if (!book) {
    throw new ApiError(404, "এই বই পাওয়া যায়নি।");
  }
  return book;
};

const assertOwnerOrAdmin = (book, user) => {
  const isOwner = book.createdBy.equals(user._id);
  // .equals() = ObjectId দুটো একই কিনা মেলায় (=== দিয়ে ObjectId মেলানো যায় না)
  if (!isOwner && user.role !== "admin") {
    throw new ApiError(403, "শুধু বইয়ের মালিক বা admin এটা করতে পারবেন।");
  }
};

const SORT_OPTIONS = {
  newest: "-createdAt",
  oldest: "createdAt",
  "price-low": "price",
  "price-high": "-price",
};
// ক্লায়েন্ট সরাসরি ফিল্ডের নাম দিতে পারবে না; শুধু এই সাদা তালিকার নামগুলো দিতে পারবে

const EDITABLE_FIELDS = ["title", "author", "price", "category", "inStock"];
// ক্লায়েন্ট যে ফিল্ডগুলো বদলাতে পারবে। createdBy বা _id কখনোই নয়

// ---------- Handler গুলো ----------

// GET /api/v1/books?category=novel&minPrice=100&q=পথের&sort=price-low&page=2&limit=5
const getBooks = async (req, res) => {
  const { category, inStock, minPrice, maxPrice, q, sort } = req.query;
  const filter = {};
  // filter = MongoDB-কে দেওয়া শর্তের object। শুরুতে খালি = সব বই

  if (category) filter.category = category;
  if (inStock !== undefined) filter.inStock = inStock === "true";
  if (minPrice || maxPrice) {
    filter.price = {};
    if (minPrice) filter.price.$gte = toNumberOrFail(minPrice, "minPrice");
    if (maxPrice) filter.price.$lte = toNumberOrFail(maxPrice, "maxPrice");
    // $gte = "এর চেয়ে বড় বা সমান", $lte = "এর চেয়ে ছোট বা সমান"
  }
  if (q) filter.title = { $regex: escapeRegex(String(q)), $options: "i" };
  // $regex + $options:"i" = বড়-ছোট হাতের ভেদাভেদ ছাড়া নামের অংশ মেলানো

  const page = Math.max(Number(req.query.page) || 1, 1);
  const limit = Math.min(Math.max(Number(req.query.limit) || 10, 1), 50);
  // page ন্যূনতম ১, limit ১ থেকে ৫০-এর মধ্যে

  const [books, total] = await Promise.all([
    Book.find(filter)
      .sort(SORT_OPTIONS[sort] || SORT_OPTIONS.newest)
      .skip((page - 1) * limit)
      .limit(limit),
    Book.countDocuments(filter),
  ]);
  // Promise.all = দুটো কাজ একসাথে শুরু করে দুটো শেষ হওয়া পর্যন্ত অপেক্ষা (একটার পর একটা করলে দ্বিগুণ সময় লাগতো)

  res.json({ success: true, page, limit, total, totalPages: Math.ceil(total / limit), data: books });
};

// GET /api/v1/books/latest
const getLatestBooks = async (req, res) => {
  const books = await Book.find().sort("-createdAt").limit(5);
  res.json({ success: true, data: books });
};

// GET /api/v1/books/:id
const getBook = async (req, res) => {
  const book = await findBookOrFail(req.params.id);
  res.json({ success: true, data: book });
};

// POST /api/v1/books
const createBook = async (req, res) => {
  const { title, author, price, category, inStock } = req.body ?? {};

  const book = await Book.create({ title, author, price, category, inStock, createdBy: req.user._id });
  // createdBy আসছে লগইন করা ব্যবহারকারীর কাছ থেকে (protect বসিয়েছে), ক্লায়েন্টের body থেকে নয় — তাহলে কেউ অন্যের নামে বই বানাতে পারতো
  // Schema-র required/enum/min নিয়ম ভাঙলে ValidationError → Error Handler → 400

  res.status(201).json({ success: true, data: book });
  // 201 Created = নতুন কিছু তৈরি হয়েছে
};

// PUT /api/v1/books/:id — পুরো বই বদলানো (সব ফিল্ড পাঠাতে হবে)
const replaceBook = async (req, res) => {
  const book = await findBookOrFail(req.params.id);
  assertOwnerOrAdmin(book, req.user);

  const body = req.body ?? {};
  book.set({
    title: body.title,
    author: body.author,
    price: body.price,
    category: body.category,
    inStock: body.inStock,
  });
  // PUT = পুরোটা প্রতিস্থাপন। যে ফিল্ড body-তে নেই সেটা undefined হয়ে মুছে যায়; title/author/price required, তাই না দিলে ValidationError

  await book.save();
  res.json({ success: true, data: book });
};

// PATCH /api/v1/books/:id — শুধু পাঠানো ফিল্ডগুলো বদলানো
const updateBook = async (req, res) => {
  const updates = {};
  for (const field of EDITABLE_FIELDS) {
    if (req.body?.[field] !== undefined) updates[field] = req.body[field];
    // req.body?.[field] = body না থাকলেও error দেবে না; থাকলে ওই ফিল্ডের মান
  }
  // updates = শুধু সাদা তালিকার ফিল্ড, যেগুলো ক্লায়েন্ট পাঠিয়েছে

  if (Object.keys(updates).length === 0) {
    throw new ApiError(400, "আপডেট করার মতো কোনো ফিল্ড পাঠানো হয়নি।");
  }

  const book = await findBookOrFail(req.params.id);
  assertOwnerOrAdmin(book, req.user);

  book.set(updates);
  await book.save();
  // .save() Schema-র validation আবার চালায়, তাই price: -5 পাঠালে আটকাবে
  res.json({ success: true, data: book });
};

// DELETE /api/v1/books/:id
const deleteBook = async (req, res) => {
  const book = await findBookOrFail(req.params.id);
  await book.deleteOne();
  res.status(204).end();
  // 204 No Content = "কাজ হয়ে গেছে, ফেরত দেওয়ার মতো কিছু নেই" — তাই body ছাড়া .end()
};

module.exports = { getBooks, getLatestBooks, getBook, createBook, replaceBook, updateBook, deleteBook };
```

### `GET /books`-এর Query — কী কী দেওয়া যায়

| Query | উদাহরণ | কাজ |
|---|---|---|
| `category` | `?category=poetry` | নির্দিষ্ট ধরনের বই |
| `inStock` | `?inStock=true` | স্টকে আছে/নেই |
| `minPrice`, `maxPrice` | `?minPrice=100&maxPrice=300` | দামের সীমা |
| `q` | `?q=পথের` | নামের অংশ দিয়ে খোঁজা |
| `sort` | `?sort=price-low` | `newest`, `oldest`, `price-low`, `price-high` |
| `page`, `limit` | `?page=2&limit=5` | পেজিনেশন (limit সর্বোচ্চ ৫০) |

সব মিলিয়ে: `GET /api/v1/books?category=novel&minPrice=100&q=দেব&sort=price-low&page=1&limit=5`

উত্তরের আকার:

```json
{
  "success": true,
  "page": 1,
  "limit": 5,
  "total": 23,
  "totalPages": 5,
  "data": [ { "_id": "...", "title": "দেবদাস", "price": 120 } ]
}
```

### কে কী করতে পারবে — অনুমতির নকশা

| কাজ | সদস্য না হলে | সাধারণ সদস্য (user) | বইয়ের মালিক | admin |
|---|---|---|---|---|
| বই দেখা | ✅ | ✅ | ✅ | ✅ |
| বই যোগ করা | ❌ 401 | ✅ | ✅ | ✅ |
| বই বদলানো (PUT/PATCH) | ❌ 401 | ❌ 403 | ✅ | ✅ |
| বই মোছা (DELETE) | ❌ 401 | ❌ 403 | ❌ 403 | ✅ |

### কয়েকটা নিরাপত্তার সিদ্ধান্ত — কোডে যা করেছি আর কেন

| সিদ্ধান্ত | কেন |
|---|---|
| `createBook`-এ `{ title, author, price, category, inStock }` **বেছে** নিলাম, পুরো `req.body` নয় | নাহলে ক্রেতা body-তে `"createdBy": "অন্যের id"` বা `"role": "admin"` ঢুকিয়ে দিতে পারতো (**Mass Assignment**) |
| `createdBy` আসে `req.user`-থেকে, body থেকে নয় | "কে বই যোগ করলো" সেটা ক্রেতার মুখের কথায় নয়, token থেকে জানবো |
| `SORT_OPTIONS`-এ সাদা তালিকা | ক্রেতা যেকোনো ফিল্ড নাম দিয়ে sort করাতে পারবে না |
| `escapeRegex(q)` | সার্চে `.*(` ধরনের চিহ্ন দিয়ে Regex ভেঙে দেওয়া (বা সার্ভার ধীর করে দেওয়া) ঠেকাতে |
| `limit` সর্বোচ্চ ৫০ | `limit=1000000` দিয়ে সব ডেটা টেনে নেওয়া ঠেকাতে |
| `toNumberOrFail` | `minPrice=abc` দিলে মিথ্যা `NaN` শর্ত না বানিয়ে পরিষ্কার 400 |
| `book.save()` (আর `findByIdAndUpdate` নয়) | `save()` Schema-র সব validation আবার চালায় |

---

## ১৫. Auth Route — সদস্য হওয়া ও লগইন

### গল্প

১ তলার **সদস্য ডেস্ক**। নতুন কেউ এসে নাম-ইমেইল-password দিয়ে সদস্য হয় (`register`), পুরোনো সদস্য এসে নিজের পরিচয় দেখিয়ে সদস্য কার্ড নেয় (`login`), আর যে কার্ড দেখাচ্ছে সে কে — সেটা জানতে চাইলে `me`-তে যায়।

### Route ফাইল

**`src/routes/auth.routes.js`**

```javascript
const express = require("express");
const { register, login, getMe } = require("../controllers/auth.controller");
const { protect } = require("../middleware/auth");

const router = express.Router();

router.post("/register", register);
router.post("/login", login);
router.get("/me", protect, getMe);
// protect = Route Middleware। /me-তে ঢোকার আগে token যাচাই হবে; /register আর /login-এ protect নেই, কারণ লগইনের আগেই তো এগুলোতে আসতে হয়

module.exports = router;
```

### Controller ফাইল

**`src/controllers/auth.controller.js`**

```javascript
const jwt = require("jsonwebtoken");
const User = require("../models/User");
const ApiError = require("../utils/ApiError");

const signToken = (userId) => {
  return jwt.sign({ id: userId }, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRES_IN || "7d",
  });
  // jwt.sign(payload, গোপন চাবি, options) = সদস্য কার্ড ছাপানো। payload-এ শুধু id রাখলাম — password বা গোপন কিছু নয়
};

const sendAuthResponse = (res, statusCode, user) => {
  res.status(statusCode).json({
    success: true,
    token: signToken(user._id),
    user: { id: user._id, name: user.name, email: user.email, role: user.role },
    // user থেকে বেছে বেছে ফিল্ড দিলাম — যাতে password hash কখনো ভুলেও বেরিয়ে না যায়
  });
};

// POST /api/v1/auth/register
const register = async (req, res) => {
  const { name, email, password } = req.body ?? {};
  // req.body ?? {} = Express 5-এ body না এলে req.body হয় undefined; ?? {} দিলে destructuring-এ error হয় না

  if (typeof name !== "string" || typeof email !== "string" || typeof password !== "string") {
    throw new ApiError(400, "name, email আর password তিনটাই লেখা (String) আকারে দিতে হবে।");
    // typeof দিয়ে String নিশ্চিত করলাম — নাহলে কেউ JSON-এ { "$ne": null } পাঠিয়ে NoSQL Injection করতে পারে
  }

  const user = await User.create({ name, email, password });
  // role পাঠাইনি — ক্লায়েন্ট নিজে নিজেকে admin বানাতে পারবে না। role সবসময় ডিফল্ট "user"
  // একই email আগে থাকলে MongoDB duplicate error (11000) দেবে — Error Handler সেটাকে 409 বানাবে

  sendAuthResponse(res, 201, user);
};

// POST /api/v1/auth/login
const login = async (req, res) => {
  const { email, password } = req.body ?? {};

  if (typeof email !== "string" || typeof password !== "string") {
    throw new ApiError(400, "email আর password দিতে হবে।");
  }

  const user = await User.findOne({ email: email.toLowerCase().trim() }).select("+password");
  // .select("+password") = মডেলে select:false দেওয়া আছে, তাই এখানে বিশেষভাবে চেয়ে নিতে হলো

  const passwordOk = user ? await user.comparePassword(password) : false;

  if (!user || !passwordOk) {
    throw new ApiError(401, "email বা password ভুল।");
    // দুই ক্ষেত্রেই একই বার্তা — "email-টা নেই" আলাদা করে বললে হ্যাকার জেনে যায় কোন email নিবন্ধিত
  }

  sendAuthResponse(res, 200, user);
};

// GET /api/v1/auth/me
const getMe = async (req, res) => {
  const { _id, name, email, role } = req.user;
  // req.user বসিয়েছে protect middleware
  res.json({ success: true, user: { id: _id, name, email, role } });
};

module.exports = { register, login, getMe };
```

### এখানকার নিরাপত্তার সিদ্ধান্তগুলো

| সিদ্ধান্ত | কেন |
|---|---|
| `typeof email !== "string"` যাচাই | JSON body-তে কেউ `{ "email": { "$ne": null }, "password": "x" }` পাঠিয়ে MongoDB-র শর্ত ঢুকিয়ে দিতে পারে — এটাই **NoSQL Injection**। String নিশ্চিত করলে আটকে যায় |
| Register-এ `role` নেওয়া হয় না | নাহলে যে কেউ নিজেকে `"role": "admin"` বানিয়ে ফেলতো। admin বানাতে হয় সরাসরি DB-তে বা আলাদা seed স্ক্রিপ্টে |
| Login-এ "email ভুল" আর "password ভুল" একই বার্তা | আলাদা বার্তা দিলে হ্যাকার জানতে পারে কোন email নিবন্ধিত |
| Password সরাসরি রাখা হয় না, `bcrypt` hash | DB চুরি গেলেও আসল password বেরোয় না |
| Response-এ `user` থেকে বেছে ফিল্ড দিলাম | password hash ভুলেও বেরিয়ে না যাক |
| Token-এ শুধু `id` | Payload যে কেউ পড়তে পারে, তাই গোপন কিছু নয় |

> **Token কোথায় রাখবে Frontend?** এই ফাইলে সহজতার জন্য `localStorage`-এ রাখা দেখানো হবে (১৯ নম্বর সেকশন)। বাস্তব প্রোডাক্টে অনেকে `httpOnly` cookie বেছে নেয়, কারণ `localStorage`-এর token XSS আক্রমণে চুরি হতে পারে। প্রতিটার সুবিধা-অসুবিধা আছে — এটা একটা আলাদা বড় আলোচনা।

---

## ১৬. Nested Route — বইয়ের ভেতরে রিভিউ

### গল্প

প্রতিটা বইয়ের নিচে একটা **রিভিউ বোর্ড** ঝোলানো আছে। রিভিউ মানেই "কোনো একটা বইয়ের রিভিউ" — বই ছাড়া রিভিউ অর্থহীন। তাই ঠিকানাটাও তেমন: **`/books/৭/reviews`** — "৭ নম্বর বইয়ের রিভিউগুলো"। একে বলে **Nested Route** — একটা resource-এর ভেতরে আরেকটা।

```
GET    /books/7/reviews              → ৭ নম্বর বইয়ের সব রিভিউ
POST   /books/7/reviews              → ৭ নম্বর বইয়ে নতুন রিভিউ
DELETE /books/7/reviews/12           → ৭ নম্বর বইয়ের ১২ নম্বর রিভিউ মোছা
```

### Route ফাইল

**`src/routes/review.routes.js`**

```javascript
const express = require("express");
const { listReviews, createReview, deleteReview } = require("../controllers/review.controller");
const { protect } = require("../middleware/auth");
const validateObjectId = require("../middleware/validateObjectId");

const router = express.Router({ mergeParams: true });
// mergeParams: true = parent router-এর path-এর :bookId এই router-এও পড়া যাবে (নাহলে req.params.bookId হতো undefined)

router.param("reviewId", validateObjectId);

router.route("/").get(listReviews).post(protect, createReview);
router.delete("/:reviewId", protect, deleteReview);

module.exports = router;
```

এই ফাইলের তিনটা মূল কথা:

1. **`express.Router({ mergeParams: true })`** — ছাড়া `req.params.bookId` `undefined` হয়ে যেতো (১০ নম্বর সেকশনের ফাঁদটা মনে করুন)।
2. **Router-এর ভেতরে path শুধু `"/"` আর `"/:reviewId"`** — `/books/:bookId/reviews` অংশটা আসে `book.routes.js`-এর `router.use("/:bookId/reviews", reviewRoutes)` থেকে।
3. **`router.param("bookId", ...)` লেখা আছে `book.routes.js`-এ**, আর `router.param("reviewId", ...)` লেখা আছে এখানে — যে Router-এর path-এ parameter-টা **সরাসরি** আছে, সে-ই তার যাচাই রাখে।

### Controller ফাইল

**`src/controllers/review.controller.js`**

```javascript
const Book = require("../models/Book");
const Review = require("../models/Review");
const ApiError = require("../utils/ApiError");

// GET /api/v1/books/:bookId/reviews
const listReviews = async (req, res) => {
  const reviews = await Review.find({ book: req.params.bookId })
    .populate("user", "name")
    .sort("-createdAt");
  // populate("user", "name") = review-এর user ফিল্ডে শুধু _id না রেখে ওই ব্যবহারকারীর name-টাও এনে বসানো

  res.json({ success: true, count: reviews.length, data: reviews });
};

// POST /api/v1/books/:bookId/reviews
const createReview = async (req, res) => {
  const book = await Book.findById(req.params.bookId);
  if (!book) {
    throw new ApiError(404, "এই বই পাওয়া যায়নি।");
  }

  const { rating, comment } = req.body ?? {};
  const review = await Review.create({ book: book._id, user: req.user._id, rating, comment });
  // একই ব্যবহারকারী একই বইয়ে দ্বিতীয়বার দিলে unique index ভাঙবে → 11000 → 409

  res.status(201).json({ success: true, data: review });
};

// DELETE /api/v1/books/:bookId/reviews/:reviewId
const deleteReview = async (req, res) => {
  const review = await Review.findOne({ _id: req.params.reviewId, book: req.params.bookId });
  // দুটো শর্তই মেলালাম: reviewId ঠিক হলেও সেটা যেন এই bookId-র নিচেরই হয়

  if (!review) {
    throw new ApiError(404, "এই রিভিউ পাওয়া যায়নি।");
  }

  const isOwner = review.user.equals(req.user._id);
  if (!isOwner && req.user.role !== "admin") {
    throw new ApiError(403, "শুধু রিভিউয়ের মালিক বা admin মুছতে পারবেন।");
  }

  await review.deleteOne();
  res.status(204).end();
};

module.exports = { listReviews, createReview, deleteReview };
```

### দুটো জিনিস খেয়াল করুন

- **`deleteReview`-এ `Review.findOne({ _id: reviewId, book: bookId })`** — শুধু `reviewId` দিয়ে খুঁজলে ৭ নম্বর বইয়ের ঠিকানায় ১২ নম্বর রিভিউ মুছে যেতে পারতো, যেটা আসলে ৯ নম্বর বইয়ের! দুটো শর্তই মেলানোয় ঠিকানার মান বজায় থাকে।
- **একই ব্যবহারকারী একই বইয়ে দুইবার রিভিউ দিতে গেলে** `Review` model-এর `unique` index ভেঙে যায় → MongoDB `11000` error দেয় → আমাদের Error Handler সেটাকে `409 Conflict` বানিয়ে ফেরত দেয় (পরের সেকশনে দেখবো)।

### Nesting কতটা গভীর হবে?

| ঠিকানা | মন্তব্য |
|---|---|
| `/books/7/reviews` | ✅ ভালো — দুই স্তর |
| `/books/7/reviews/12` | ✅ চলে — তিন স্তর, তবে এর বেশি না |
| `/authors/3/books/7/reviews/12/likes/5` | ❌ অনেক গভীর — পড়া, লেখা, Router সাজানো সবই কঠিন |

> **নিয়ম:** ঠিকানা তিন স্তরের বেশি গভীর হলে বুঝতে হবে কোনো একটা resource-কে আলাদা ঠিকানা দেওয়া উচিত। যেমন `/reviews/12/likes` (রিভিউয়ের `_id` তো যেকোনোভাবেই আলাদা)।

---

## ১৭. Error Handling — ৪০৪ আর অভিযোগ ডেস্ক

### গল্প

মলে দুই রকম গোলমাল হতে পারে, আর দুটোর জন্য আলাদা ব্যবস্থা:

1. **"এই নামে কোনো দোকানই নেই"** — ক্রেতা এমন ঠিকানা চাইলো, যেটা বোর্ডে নেই। কারও দোষ নেই, শুধু জানিয়ে দিতে হবে → **৪০৪ Handler**।
2. **"কর্মী কাজ করতে গিয়ে গোলমাল করলেন"** — গুদামের তালা ভাঙা, ক্রেতার ফর্মে ভুল, অনুমতি নেই → **অভিযোগ ডেস্ক (Error Handler)**।

দুটোই থাকবে **সব Route-এর পরে**, আর শেষেরটা থাকবে **একদম শেষে**।

### ১. ৪০৪ Handler

**`src/middleware/notFound.js`**

```javascript
// কোনো route না মিললে request এখানে এসে পড়ে — তাই app.js-এ এটা সব route-এর পরে বসে
const notFound = (req, res) => {
  res.status(404).json({
    success: false,
    message: `${req.method} ${req.originalUrl} — এই route আমাদের সার্ভারে নেই।`,
  });
};

module.exports = notFound;
```

এটা সাধারণ Middleware (তিন parameter)। কোনো Route না মিললে Request তালিকা ঘুরে ঘুরে এখানে এসে পড়ে — তাই এটা সব Route-এর **পরে** বসাতে হয়।

### ২. Error Handler — চার parameter, নাহলে চিনবে না

**`src/middleware/errorHandler.js`**

```javascript
// Error-handling middleware = চারটা parameter (err, req, res, next)। Express এই চার parameter দেখেই বোঝে এটা অভিযোগ ডেস্ক
const errorHandler = (err, req, res, next) => {
  if (res.headersSent) {
    return next(err);
    // উত্তর অর্ধেক চলে গেছে — এখন আর নতুন করে JSON পাঠানো যায় না, Express-এর ডিফল্ট handler-এর হাতে ছেড়ে দিই
  }

  let statusCode = err.statusCode || err.status || 500;
  // ApiError-এ statusCode থাকে, body-parser-এর error-এ status থাকে, বাকি সব অজানা error = 500
  let message = err.message || "সার্ভারে সমস্যা হয়েছে।";

  if (err.name === "ValidationError") {
    // Mongoose schema-র নিয়ম ভাঙলে (required, min, enum...)
    statusCode = 400;
    message = Object.values(err.errors)
      .map((e) => (e.name === "CastError" ? `"${e.path}" এর মান সঠিক নয়।` : e.message))
      .join(", ");
    // ভেতরে কোনো ফিল্ডের type ভুল হলে (price: "abc") সেটাও এখানে CastError হয়ে থাকে — তার Mongoose-এর ইংরেজি বার্তার বদলে নিজের বাংলা বার্তা দিলাম
  } else if (err.name === "CastError") {
    // Mongoose কোনো query-র মান ঠিক type-এ বদলাতে পারলো না (যেমন filter-এ ভুল ধরনের মান)
    statusCode = 400;
    message = `"${err.path}" এর মান সঠিক নয়।`;
  } else if (err.code === 11000) {
    // MongoDB duplicate key — unique নিয়ম ভাঙা (একই email, একই review)
    statusCode = 409;
    message = `${Object.keys(err.keyValue || {}).join(", ")} আগে থেকেই আছে।`;
  } else if (err.name === "JsonWebTokenError" || err.name === "TokenExpiredError") {
    statusCode = 401;
    message = "token সঠিক নয় বা মেয়াদ শেষ। আবার লগইন করুন।";
  } else if (err.type === "entity.parse.failed") {
    // express.json() ভাঙা JSON পেলে এই type-এর error দেয়
    statusCode = 400;
    message = "JSON ফরম্যাট ভুল — কমা, কোটেশন বা ব্র্যাকেট দেখুন।";
  }

  if (statusCode === 500) {
    console.error("💥 অপ্রত্যাশিত error:", err);
    if (process.env.NODE_ENV === "production") {
      message = "সার্ভারে সমস্যা হয়েছে। একটু পরে আবার চেষ্টা করুন।";
      // production-এ ভেতরের error বার্তা ক্লায়েন্টকে দেখাতে নেই — হ্যাকারের সুবিধা হয়
    }
  }

  res.status(statusCode).json({ success: false, message });
};

module.exports = errorHandler;
```

> ⚠️ **Express কীভাবে চেনে এটা Error Handler?** ফাংশনে **ঠিক চারটা parameter** `(err, req, res, next)` দেখে। `next` ব্যবহার না করলেও লিখতেই হবে — তিনটা লিখলে সেটা সাধারণ Middleware হয়ে যায় আর error কখনো তার কাছে পৌঁছায় না।

### Error কীভাবে Error Handler-এ পৌঁছায়

```mermaid
flowchart TD
    subgraph SRC["💥 ভুলের উৎস"]
        S1["throw new ApiError(404, ...)<br/>(আমাদের নিজের)"]
        S2["Mongoose:<br/>ValidationError, CastError,<br/>duplicate key 11000"]
        S3["jwt.verify():<br/>JsonWebTokenError,<br/>TokenExpiredError"]
        S4["express.json():<br/>ভাঙা JSON"]
        S5["অপ্রত্যাশিত bug<br/>(TypeError ইত্যাদি)"]
    end
    SRC --> EH["🛎️ Error Handler<br/>(err, req, res, next)"]
    EH --> Q{"কোন ধরনের error?"}
    Q -- "ApiError / body-parser" --> R1["তার নিজের statusCode"]
    Q -- "ValidationError / CastError" --> R2["400 + বাংলা বার্তা"]
    Q -- "code 11000" --> R3["409 Conflict"]
    Q -- "JWT error" --> R4["401 Unauthorized"]
    Q -- "বাকি সব" --> R5["500 + লগে পুরো error<br/>(production-এ বার্তা লুকানো)"]
    R1 --> J["📤 { success: false, message }"]
    R2 --> J
    R3 --> J
    R4 --> J
    R5 --> J
```

### Async Handler-এর error — Express 4 আর 5-এর বড় পার্থক্য

```javascript
router.get("/:id", async (req, res) => {
  const book = await Book.findById(req.params.id);
  throw new Error("বুম!");
  // Express 5 → error নিজে থেকেই Error Handler-এ চলে যায় ✅
  // Express 4 → error কেউ ধরে না, Request ঝুলে থাকে (আর কখনো কখনো সার্ভারই crash করে) ❌
});
```

আপনি যদি কখনো **Express 4** প্রজেক্টে কাজ করেন, তাহলে এই ছোট wrapper লাগবে (বা `express-async-errors` প্যাকেজ):

```javascript
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
  // fn চালাও; সেটা যদি reject করে (async-এ throw), সেই error-কে next(err) হিসেবে Error Handler-এ ঠেলে দাও
};

router.get("/", asyncHandler(controller.getBooks));
// Express 4-এ প্রতিটা async Controller এভাবে মুড়ে দিতে হয়
```

আমাদের এই ফাইলে Express 5 ধরা হয়েছে, তাই `asyncHandler` লাগে না — Controller-এ সরাসরি `throw new ApiError(...)` লিখতে পারছি।

### বাস্তবে কী কী উত্তর আসবে

| ক্রেতা কী করলো | Error-র উৎস | Status | `message` |
|---|---|---|---|
| ভাঙা JSON পাঠালো (`{bad`) | `express.json()` | `400` | JSON ফরম্যাট ভুল — কমা, কোটেশন বা ব্র্যাকেট দেখুন। |
| `POST /books` — title নেই | Mongoose `ValidationError` | `400` | বইয়ের নাম দিতে হবে |
| `POST /books` — `price: "abc"` | Mongoose (ভেতরে `CastError`) | `400` | "price" এর মান সঠিক নয়। |
| `POST /books` — `price: -5` | Mongoose `ValidationError` | `400` | দাম ঋণাত্মক হতে পারে না |
| `GET /books/12345` | `router.param` → `ApiError` | `400` | "12345" একটা সঠিক id নয়। |
| `GET /books/<নেই এমন সঠিক id>` | Controller → `ApiError` | `404` | এই বই পাওয়া যায়নি। |
| token ছাড়া `POST /books` | `protect` → `ApiError` | `401` | লগইন করা নেই। … |
| মেয়াদ-শেষ token | `jwt.verify` | `401` | token সঠিক নয় বা মেয়াদ শেষ। … |
| user হয়ে `DELETE /books/:id` | `restrictTo` → `ApiError` | `403` | এই কাজের অনুমতি আপনার নেই। |
| একই email দিয়ে আবার register | MongoDB `11000` | `409` | email আগে থেকেই আছে। |
| একই বইয়ে দ্বিতীয় রিভিউ | MongoDB `11000` | `409` | book, user আগে থেকেই আছে। |
| `GET /api/v1/nothing` | `notFound` | `404` | GET /api/v1/nothing — এই route আমাদের সার্ভারে নেই। |
| DB ভেঙে গেল / কোডে bug | অজানা | `500` | (production-এ) সার্ভারে সমস্যা হয়েছে। … |

> **ভালো অভ্যাস:** ক্লায়েন্টকে সবসময় একই ছাঁচে (`{ success: false, message }`) উত্তর দিন, আর `500`-এর ভেতরের আসল কারণ (stack trace, DB-র নাম) কখনো প্রোডাকশনে বাইরে দেখাবেন না — শুধু সার্ভারের লগে রাখুন।

---

## ১৮. `app.js` ও `server.js` — সব জোড়া লাগানো

### গল্প

সব তলা ম্যানেজার তৈরি। এবার মলের প্রধান ম্যানেজার (`app.js`) সবাইকে নিজ নিজ জায়গায় বসিয়ে দেবেন, আর মালিক (`server.js`) গুদামের চাবি লাগিয়ে মলের দরজা খুলবেন।

### সব Router-এর জোড়ার জায়গা

**`src/routes/index.js`**

```javascript
const express = require("express");
const authRoutes = require("./auth.routes");
const bookRoutes = require("./book.routes");

const router = express.Router();

router.get("/health", (req, res) => {
  res.json({ success: true, status: "ok", uptime: process.uptime() });
  // health check = "সার্ভার বেঁচে আছে?" — hosting/monitoring টুল এই route-এ ping করে
});

router.use("/auth", authRoutes);
router.use("/books", bookRoutes);

module.exports = router;
```

### `app.js` — অ্যাপ সাজানো

**`src/app.js`**

```javascript
const express = require("express");
const cors = require("cors");
const logger = require("./middleware/logger");
const routes = require("./routes");
const notFound = require("./middleware/notFound");
const errorHandler = require("./middleware/errorHandler");

const app = express();

app.disable("x-powered-by");
// প্রতিটা উত্তরে "X-Powered-By: Express" লেবেল লাগানো বন্ধ — বাইরের লোককে আমাদের টুল জানিয়ে লাভ নেই

// ---------- Application Middleware (ক্রম গুরুত্বপূর্ণ) ----------
app.use(cors({ origin: process.env.CLIENT_URL, credentials: true }));
// cors = অন্য ঠিকানা (React: localhost:5173) থেকে আসা request-কে ঢুকতে দেওয়ার অনুমতিপত্র
app.use(logger);
app.use(express.json({ limit: "1mb" }));
// express.json() = JSON body পড়ে req.body-তে বসায়। অবশ্যই route-এর আগে

// ---------- Route ----------
app.use("/api/v1", routes);
// সব API route এখন /api/v1 দিয়ে শুরু। ভবিষ্যতে /api/v2 আলাদা বানানো যাবে

// ---------- সবার শেষে ----------
app.use(notFound);
app.use(errorHandler);

module.exports = app;
```

### `server.js` — DB সংযোগ আর দরজা খোলা

**`server.js`**

```javascript
require("dotenv").config();
// dotenv = .env ফাইল পড়ে process.env-এ বসায়। এটা সবার আগে চালাতে হয়, নাহলে বাকি ফাইলে process.env.XXX খালি পাওয়া যাবে

const app = require("./src/app");
const connectDB = require("./src/config/db");

const PORT = process.env.PORT || 5000;

connectDB()
  .then(() => {
    app.listen(PORT, () => console.log(`🚀 সার্ভার চালু → http://localhost:${PORT}/api/v1/health`));
    // আগে DB সংযুক্ত হলো, তারপর দরজা খুললাম — DB ছাড়া চালু হওয়া সার্ভার কোনো কাজের নয়
  })
  .catch((err) => {
    console.error("❌ MongoDB সংযোগ ব্যর্থ:", err.message);
    process.exit(1);
    // process.exit(1) = ভুল নিয়ে বন্ধ হওয়া (0 মানে সফলভাবে বন্ধ)
  });
```

### `app.js`-এর সঠিক সাজানোর ক্রম

```
1. app.disable("x-powered-by")
2. Application Middleware ...... cors → logger → express.json()
3. Route ......................  app.use("/api/v1", routes)
4. ৪০৪ Handler ................  app.use(notFound)
5. Error Handler ..............  app.use(errorHandler)      ← সবার শেষে
```

### চালানো

```bash
# ১. MongoDB চালু আছে কিনা নিশ্চিত করুন (লোকাল mongod বা Atlas)
# ২. প্রজেক্টের root থেকে:
npm run dev
# ✅ MongoDB সংযুক্ত হয়েছে
# 🚀 সার্ভার চালু → http://localhost:5000/api/v1/health
```

ব্রাউজারে `http://localhost:5000/api/v1/health` খুললে `{"success":true,"status":"ok",...}` দেখা গেলে মল খুলে গেছে।

### একটা `PATCH /books/:id` Request-এর পুরো যাত্রা

```mermaid
sequenceDiagram
    autonumber
    participant C as 👤 Client
    participant AM as app.js Middleware
    participant IDX as routes/index.js
    participant BR as book.routes.js
    participant PR as protect
    participant CT as updateBook Controller
    participant DB as 🗄️ MongoDB
    participant EH as Error Handler

    C->>AM: PATCH /api/v1/books/7 + Bearer token + JSON
    AM->>AM: cors, logger শুরু, express.json() body পড়লো
    AM->>IDX: app.use("/api/v1") মিললো
    IDX->>BR: router.use("/books") মিললো
    BR->>BR: router.param("id") — ফরম্যাট যাচাই
    BR->>PR: PATCH /:id → প্রথম Middleware
    PR->>DB: token থেকে user খোঁজা
    DB-->>PR: user
    PR->>CT: req.user বসিয়ে next()
    CT->>DB: বই খুঁজে, মালিক যাচাই, set + save
    DB-->>CT: আপডেট হওয়া বই
    CT-->>C: 200 OK + { success: true, data: বই }
    Note over PR,EH: যেকোনো ধাপে throw হলে সোজা Error Handler → 4xx/5xx JSON
    AM->>AM: logger finish: PATCH /api/v1/books/7 → 200 (18ms)
```

---

## ১৯. React থেকে Route ডাকা — CORS, fetch আর Deploy

### গল্প

তিথির অ্যাপ (React) আর স্বপ্ননগর মল (Express) **আলাদা বাড়িতে**। অ্যাপ চলে `localhost:5173` (Vite) আর মল চলে `localhost:5000`। ব্রাউজারের নিয়ম হলো: **এক বাড়ির অ্যাপ অন্য বাড়িতে সরাসরি ঢুকতে চাইলে, ওই বাড়ির অনুমতি লাগবে।** এই নিয়মের নাম **Same-Origin Policy**, আর অনুমতিটার নাম **CORS** (Cross-Origin Resource Sharing)।

> **Origin** = Protocol + Host + Port। `http://localhost:5173` আর `http://localhost:5000` — শুধু Port আলাদা হলেও **আলাদা Origin**।

### CORS error কেমন দেখতে

Browser Console-এ লাল রঙে:

```
Access to fetch at 'http://localhost:5000/api/v1/books' from origin 'http://localhost:5173'
has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present...
```

> ⚠️ এটা **সার্ভারের Route-এর ভুল নয়**, আর React কোড বদলে এটা ঠিক হয় না। সার্ভারকে বলতে হয় "এই অ্যাপকে ঢুকতে দাও"। Postman-এ কখনো CORS error আসে না, কারণ CORS শুধু **ব্রাউজারের** নিয়ম।

### Preflight — ঢোকার আগে "অনুমতি আছে?" জিজ্ঞেস

`Authorization` header বা `application/json` Body সহ Request পাঠানোর আগে ব্রাউজার নিজে থেকে আগে একটা **OPTIONS** Request পাঠায় — "আমি এই ঠিকানায়, এই Method নিয়ে আসতে চাই, অনুমতি আছে?" সার্ভার সম্মতি দিলে তবেই আসল Request যায়। `cors` middleware এই OPTIONS-এর উত্তর নিজে দিয়ে দেয়।

```mermaid
sequenceDiagram
    autonumber
    participant B as ⚛️ Browser (localhost:5173)
    participant S as 🛎️ Express (localhost:5000)
    B->>S: OPTIONS /api/v1/books (preflight) — Origin: localhost:5173
    S-->>B: 204 + Access-Control-Allow-Origin: localhost:5173 (cors middleware)
    Note over B: অনুমতি আছে, তাই আসল Request যাচ্ছে
    B->>S: POST /api/v1/books + Authorization + JSON
    S-->>B: 201 Created + Access-Control-Allow-Origin
```

আমাদের `app.js`-এ যা লেখা আছে:

```javascript
app.use(cors({ origin: process.env.CLIENT_URL, credentials: true }));
// origin: শুধু আমাদের React অ্যাপের ঠিকানাকে ঢুকতে দাও (.env-এ CLIENT_URL=http://localhost:5173)
// ⚠️ app.use(cors()) একা লিখলে সব ঠিকানাকে অনুমতি দেওয়া হয় — শেখার সময় ঠিক আছে, প্রোডাকশনে নয়
// credentials: true = cookie বা Authorization সহ Request চলবে
```

### বিকল্প: Vite Proxy (শুধু Development-এ)

CORS-এর ঝামেলা এড়াতে React-এর dev server-কেই বলে দেওয়া যায়: `/api` দিয়ে শুরু সব Request তুমি সার্ভারে ঠেলে দাও। ব্রাউজারের চোখে তখন সবই এক Origin।

**`vite.config.js`** (React প্রজেক্টে)

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      "/api": "http://localhost:5000",
      // React থেকে fetch("/api/v1/books") করলে Vite সেটা নিজে http://localhost:5000/api/v1/books-এ পাঠাবে
    },
  },
});
```

### React-এর `api.js` — সব Route ডাকার এক জায়গা

**`src/api.js`** (React প্রজেক্টে)

```javascript
const API_URL = import.meta.env.VITE_API_URL || "/api/v1";
// Vite proxy চালালে "/api/v1"-ই যথেষ্ট। সরাসরি সার্ভারে গেলে .env-এ VITE_API_URL=http://localhost:5000/api/v1

export const getToken = () => localStorage.getItem("token");
export const setToken = (token) => {
  if (token) localStorage.setItem("token", token);
  else localStorage.removeItem("token");
  // token থাকলে রাখো, না থাকলে (লগআউট) মুছে দাও
};

async function request(path, { method = "GET", body, query } = {}) {
  const cleanQuery = Object.fromEntries(
    Object.entries(query ?? {}).filter(([, value]) => value !== "" && value != null)
  );
  // খালি বা undefined query বাদ — নাহলে ?q=undefined চলে যেতো

  const queryString = Object.keys(cleanQuery).length ? "?" + new URLSearchParams(cleanQuery) : "";
  // URLSearchParams = key=value জোড়া সুন্দরভাবে বানায়; বাংলা/স্পেস নিজে থেকে এনকোড করে

  const headers = {};
  if (body !== undefined) headers["Content-Type"] = "application/json";
  // Body পাঠালে Content-Type বলে দিতে হয়, নাহলে সার্ভারের express.json() সেটা পড়বে না

  const token = getToken();
  if (token) headers.Authorization = `Bearer ${token}`;
  // সদস্য কার্ড থাকলে প্রতিটা Request-এই দেখাবো

  const response = await fetch(`${API_URL}${path}${queryString}`, {
    method,
    headers,
    body: body !== undefined ? JSON.stringify(body) : undefined,
  });

  if (response.status === 204) return null;
  // 204 No Content-এ কোনো body নেই, response.json() করলে error দিতো

  const data = await response.json().catch(() => null);

  if (!response.ok) {
    throw new Error(data?.message || `Request ব্যর্থ (${response.status})`);
    // fetch নিজে 4xx/5xx-এ error ছোঁড়ে না! response.ok দেখে নিজেই ছুঁড়তে হয়
    // আমাদের সার্ভার সবসময় { success: false, message } পাঠায়, তাই message দিয়েই সুন্দর error বার্তা পাওয়া যায়
  }

  return data;
}

export const api = {
  register: (data) => request("/auth/register", { method: "POST", body: data }),
  login: (data) => request("/auth/login", { method: "POST", body: data }),
  me: () => request("/auth/me"),

  getBooks: (query) => request("/books", { query }),
  getBook: (id) => request(`/books/${id}`),
  createBook: (book) => request("/books", { method: "POST", body: book }),
  updateBook: (id, changes) => request(`/books/${id}`, { method: "PATCH", body: changes }),
  deleteBook: (id) => request(`/books/${id}`, { method: "DELETE" }),

  getReviews: (bookId) => request(`/books/${bookId}/reviews`),
  addReview: (bookId, review) => request(`/books/${bookId}/reviews`, { method: "POST", body: review }),
};
// এখন পুরো React অ্যাপে শুধু api.getBooks({ page: 2 }) লিখলেই হবে — URL, header, token নিয়ে আর ভাবতে হবে না
```

### লগইন — token সংরক্ষণ

```javascript
import { api, setToken } from "./api";

async function handleLogin(email, password) {
  const result = await api.login({ email, password });
  // result = { success: true, token: "...", user: {...} } — POST /api/v1/auth/login-এর উত্তর
  setToken(result.token);
  // token এখন localStorage-এ। এরপর থেকে api.js নিজে থেকে প্রতিটা Request-এ জুড়ে দেবে
  return result.user;
}

function handleLogout() {
  setToken(null);
  // token মুছে দিলেই "লগআউট" (সার্ভারের কিছু করার নেই — JWT নিজে থেকেই মেয়াদ শেষে অকেজো হয়)
}
```

### React কম্পোনেন্ট — পেজিনেশন আর সার্চ সহ বইয়ের তালিকা

```jsx
import { useEffect, useState } from "react";
import { api } from "./api";

export default function BookList() {
  const [books, setBooks] = useState([]);
  const [page, setPage] = useState(1);
  const [totalPages, setTotalPages] = useState(1);
  const [search, setSearch] = useState("");
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState("");

  useEffect(() => {
    let ignore = false;
    // ignore = পুরোনো Request-এর উত্তর দেরিতে এলে সেটা যেন নতুন উত্তরের ওপর না বসে যায় (race condition ঠেকানো)

    setLoading(true);
    api
      .getBooks({ q: search, page, limit: 5 })
      // এটা আসলে GET /api/v1/books?q=...&page=...&limit=5
      .then((result) => {
        if (ignore) return;
        setBooks(result.data);            // সার্ভারের { success, page, total, totalPages, data } থেকে data
        setTotalPages(result.totalPages);
        setError("");
      })
      .catch((err) => !ignore && setError(err.message))
      .finally(() => !ignore && setLoading(false));

    return () => {
      ignore = true;
      // ক্লিনআপ: search বা page বদলালে আগের Request-এর উত্তর আর গ্রহণ করবো না
    };
  }, [search, page]);
  // [search, page] = এদের কোনোটা বদলালেই আবার সার্ভারকে জিজ্ঞেস করবে

  return (
    <div>
      <input
        value={search}
        onChange={(e) => {
          setSearch(e.target.value);
          setPage(1); // নতুন সার্চে ১ নম্বর পাতা থেকে শুরু
        }}
        placeholder="বইয়ের নাম খুঁজুন..."
      />

      {loading && <p>লোড হচ্ছে...</p>}
      {error && <p style={{ color: "red" }}>{error}</p>}

      <ul>
        {books.map((book) => (
          <li key={book._id}>
            {book.title} — {book.author} (৳{book.price})
          </li>
        ))}
      </ul>

      <button disabled={page <= 1} onClick={() => setPage(page - 1)}>আগের পাতা</button>
      <span> {page} / {totalPages} </span>
      <button disabled={page >= totalPages} onClick={() => setPage(page + 1)}>পরের পাতা</button>
    </div>
  );
}
```

> এই উদাহরণে প্রতিটা অক্ষর টাইপ করলেই সার্ভারে Request যাবে। বাস্তবে সার্চে **debounce** (থামার ৩০০ms পরে Request) দেওয়া হয়। এটা React-এর বিষয়, তাই এখানে আর বাড়ালাম না।

### React-এর কাজ ↔ Backend Route — এক নজরে মিলিয়ে নিন

| ব্যবহারকারী যা করলো | React-এ | HTTP Request | Express Route |
|---|---|---|---|
| বইয়ের পাতা খুললো | `api.getBooks(...)` | `GET /books?page=1` | `router.route("/").get(getBooks)` |
| বইয়ের বিস্তারিত | `api.getBook(id)` | `GET /books/:id` | `router.route("/:id").get(getBook)` |
| লগইন ফর্ম জমা দিলো | `api.login(...)` | `POST /auth/login` | `router.post("/login", login)` |
| "নতুন বই যোগ করুন" | `api.createBook(...)` | `POST /books` + token | `.post(protect, createBook)` |
| "দাম বদলান" | `api.updateBook(id, {price})` | `PATCH /books/:id` + token | `.patch(protect, updateBook)` |
| "মুছুন" (admin) | `api.deleteBook(id)` | `DELETE /books/:id` + token | `.delete(protect, restrictTo("admin"), deleteBook)` |
| রিভিউ লিখলো | `api.addReview(id, {...})` | `POST /books/:bookId/reviews` | `.post(protect, createReview)` |

### Axios ব্যবহার করতে চাইলে

`fetch`-এর বদলে `axios` ব্যবহার করলে `api.js` আরও ছোট হয় — 4xx/5xx-এ নিজে থেকেই error ছোঁড়ে, আর JSON নিজে বানিয়ে-পড়ে:

```javascript
import axios from "axios";

const http = axios.create({ baseURL: import.meta.env.VITE_API_URL || "/api/v1" });
// baseURL = প্রতিটা Request-এর সামনে এই ঠিকানা নিজে বসে যাবে

http.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
  // interceptor = প্রতিটা Request যাওয়ার ঠিক আগে এই ফাংশন চলে — token বসানোর সবচেয়ে সহজ জায়গা
});

export const getBooks = (params) => http.get("/books", { params }).then((res) => res.data);
// axios-এ query দিতে { params } — নিজেই ?key=value বানিয়ে দেয়
```

### Production: React-কেও Express থেকে পরিবেশন

Deploy-এর সময় অনেকেই React (`npm run build` করে যে `dist` ফোল্ডার পায়) আর Express একই সার্ভার থেকে চালায়। তখন CORS-ই লাগে না, কারণ সব একই Origin। কিন্তু একটা ফাঁদ আছে — **২ নম্বর সেকশনে যা বলেছিলাম**: ব্যবহারকারী `/books` (React-এর পেজ) লিখে Refresh দিলে ব্রাউজার সার্ভারে `GET /books` পাঠায়, আর সার্ভার তো ওটা চেনে না → 404। সমাধান: **"বাকি সব GET Request-এ React-এর `index.html` পাঠিয়ে দাও"** — তারপর React Router নিজেই বুঝে নেবে কোন পেজ দেখাতে হবে।

`src/app.js`-এ Route-এর পরে, Error Handler-এর আগে:

```javascript
const path = require("path");

app.use("/api/v1", routes);
app.use("/api", notFound);
// /api দিয়ে শুরু কিন্তু মেলেনি এমন সব Request → JSON 404 (React-এর index.html নয়!)

const clientBuild = path.join(__dirname, "..", "client", "dist");
// clientBuild = React প্রজেক্টের build ফোল্ডারের পূর্ণ ঠিকানা (client/dist)

app.use(express.static(clientBuild));
// build-এর ফাইল (JS, CSS, ছবি) সরাসরি দিয়ে দাও

app.get("/{*splat}", (req, res) => {
  res.sendFile(path.join(clientBuild, "index.html"));
  // Express 5-এ "সব ধরার" Route এভাবে লিখতে হয়। Express 4-এ লেখা হতো app.get("*", ...)
  // /{*splat} root ("/") সহ সব GET ধরে — React Router পরে ঠিক করবে কোন পেজ দেখাবে
});

app.use(errorHandler);
```

আর যদি React আলাদা হোস্টে (যেমন Vercel/Netlify) আর Express আলাদা হোস্টে (Render/Railway) থাকে — তাহলে CORS-এ `CLIENT_URL`-এ React-এর আসল ঠিকানা (`https://booknest.vercel.app`) বসাতে হবে।

---

## ২০. REST নকশার নিয়ম — Route-এর নাম কেমন হবে

### গল্প

কল্পনা করুন, স্বপ্ননগর মলের ডিরেক্টরি বোর্ডটা এমন:

```
৩ তলা — বই দেখাও
৩ তলা — বই ফেলে দাও ৭
৩ তলা — নতুনবই_যোগ
৩ তলা — Book7-দাম-বদল-করুন
```

কোন বোর্ডে কোনটা লেখা, কে লিখেছে, কোন নিয়মে — কিছুই বোঝা যায় না। নতুন কর্মী এলে তাকে প্রতিটা লাইন মুখস্থ করতে হবে। এবার ভালো মলের বোর্ড দেখুন:

```
৩ তলা / বইঘর       →  দেখা | জমা দেওয়া
৩ তলা / বইঘর / ৭   →  দেখা | বদলানো | সরানো
```

এখানে **ঠিকানায় শুধু জায়গার নাম** (বিশেষ্য), আর **কাজটা আলাদা** (ক্রিয়া)। একটা তলার নিয়ম বুঝলে বাকি সব তলা আপনা-আপনি বোঝা যায়। এই সংস্কৃতির নামই **REST**।

> **REST-এর মূল কথা মাত্র একটা:** URL বলবে **কোন জিনিস** (resource) নিয়ে কথা হচ্ছে — বিশেষ্য। HTTP Method বলবে সেটা নিয়ে **কী করা হবে** — ক্রিয়া। নামটার পুরো রূপ (Representational State Transfer) মুখস্থ করার দরকার নেই।

### ভুল ও শুদ্ধ — পাশাপাশি

| ❌ ভুল (ক্রিয়া ঢুকে গেছে) | ✅ শুদ্ধ | কেন |
|---|---|---|
| `GET /getAllBooks` | `GET /books` | `get` তো Method-ই বলে দিচ্ছে |
| `POST /createBook` | `POST /books` | `create` মানে POST |
| `GET /getBookById/7` | `GET /books/7` | `:id` দিয়েই তো আলাদা করা যায় |
| `POST /updateBook/7` | `PATCH /books/7` | বদলানোর জন্য PUT/PATCH আছে |
| `GET /deleteBook/7` | `DELETE /books/7` | 💀 GET দিয়ে মুছলে সার্চ ইঞ্জিন বা browser-এর prefetch লিংক খুলেই বই মুছে ফেলতে পারে |
| `GET /book/7` (একবচন) | `GET /books/7` | সবখানে বহুবচন রাখলে মনে রাখতে হয় না কোনটা কোন রূপ |
| `GET /books?id=7` | `GET /books/7` | একটা নির্দিষ্ট জিনিসের পরিচয় (id) থাকে path-এ, query-তে নয় |
| `GET /Book_Reviews` | `GET /book-reviews` | ছোট হাতের অক্ষর আর হাইফেন |
| `GET /books/7/reviews/3/comments/9/likes` | `GET /likes/:id` বা query দিয়ে ফিল্টার | ৩ স্তরের বেশি নয় (১৬ নম্বর সেকশন) |

### দশটা নিয়ম, এক নজরে

| # | নিয়ম | কেন | BookNest-এ যেখানে দেখেছি |
|---|---|---|---|
| ১ | URL-এ **বহুবচন বিশেষ্য** (`/books`, `/users`) | Collection আর তার ভেতরের এক-একটা (`/books/7`) — দুটোই একই ছাঁচে পড়ে | `/api/v1/books` |
| ২ | URL-এ **ক্রিয়া নয়** | ক্রিয়া বহন করে HTTP Method | `POST /books`, `DELETE /books/:id` |
| ৩ | **ছোট হাতের অক্ষর**, শব্দ জোড়া দিতে `-` | কিছু সার্ভার/CDN path-এর বড়-ছোট হাতের অক্ষর আলাদা ধরে। Express ডিফল্টে ধরে না (৫ নম্বর সেকশনে দেখেছি), কিন্তু সবখানে একই আচরণ আশা করা ঠিক নয় | `/books/latest` |
| ৪ | **মালিকানা = Nesting**, তবে গভীরে নয় | রিভিউ বই ছাড়া অর্থহীন, তাই `/books/:bookId/reviews`। ৩ স্তরের বেশি হলে পড়া কঠিন | `review.routes.js` |
| ৫ | **ফিল্টার, সাজানো, পেজ = Query** | এগুলো "কোন জিনিস" নয়, "কীভাবে দেখাবো" — তাই `?category=novel&sort=price-low&page=2` | `GET /books` |
| ৬ | **সঠিক Method + সঠিক Status** | Client (React, মোবাইল অ্যাপ) status code দেখেই সিদ্ধান্ত নেয় | ৭ নম্বর সেকশনের তালিকা |
| ৭ | **Version আগে থেকেই** (`/api/v1`) | ভবিষ্যতে ছাঁচ বদলাতে হলে `/api/v2` বানাও, পুরোনো অ্যাপ ভাঙবে না | `app.use("/api/v1", routes)` |
| ৮ | **সবসময় একই ছাঁচে উত্তর** | Frontend-এ একটাই `if (data.success)` দিয়ে সব সামলানো যায় | `{ success, data }` / `{ success, message }` |
| ৯ | **খালি তালিকা মানে 404 নয়** | `GET /books` করে কোনো বই না পেলে জবাব `200` আর `data: []`। 404 মানে "এই ঠিকানাই নেই" — কিন্তু `/books` ঠিকানাটা তো আছে, শুধু ভেতরে কিছু নেই | `getBooks` |
| ১০ | **আগে Route Table লিখুন, তারপর কোড** | কোন ঠিকানায় কী কাজ, কার অনুমতি, কী কী ব্যর্থতা — কাগজে ঠিক করে নিলে কোডে ঘুরে দাঁড়াতে হয় না | ১৩ নম্বর সেকশন |

### যেসব কাজ CRUD-এ পড়ে না — তখন কী করবেন?

সব কাজ তো "তৈরি, দেখা, বদলানো, মোছা" নয়। লগইন, লগআউট, বই "প্রকাশ" করা, অর্ডার "বাতিল" করা — এগুলো কী?

| পরিস্থিতি | সাধারণ সমাধান | উদাহরণ |
|---|---|---|
| কাজটা আসলে **অবস্থা বদল** | `PATCH` দিয়ে state ফিল্ড বদলান | `PATCH /orders/12` আর body `{ "status": "cancelled" }` |
| কাজটা একটা **প্রক্রিয়া চালু করে** (ইমেইল যায়, পেমেন্ট হয়) | জিনিসের নিচে একটা ক্রিয়া-পথ, Method `POST` | `POST /books/7/publish`, `POST /orders/12/cancel` |
| কাজটা **সেশন/পরিচয়** নিয়ে | `/auth` নামের আলাদা এলাকা | `POST /auth/login`, `GET /auth/me` |
| "আমার নিজের" তথ্য | `me` নামে একটা স্থির path | `GET /auth/me` — মনে আছে? **স্থির path সবসময় `:id`-এর আগে** |

> এখানে কোনো "একমাত্র সঠিক" উত্তর নেই — বাস্তব প্রজেক্টে এসব নিয়ে তর্ক হয়। আসল নিয়ম: **নিজের প্রজেক্টে একটা ধরন বেছে নিন, তারপর সবখানে সেটাই মানুন।**

### নাম ঠিক করার সিদ্ধান্ত-ছক

```mermaid
flowchart TD
    A["নতুন একটা Route লিখবো"] --> B{"কাজটা কি একটা<br/>জিনিস তৈরি / দেখা / বদলানো / মোছা?"}
    B -- "হ্যাঁ" --> C["URL = জিনিসের বহুবচন নাম<br/>Method = কাজ<br/>যেমন: DELETE /books/7"]
    B -- "না" --> D{"কাজটা কি শুধু<br/>একটা অবস্থা বদল?"}
    D -- "হ্যাঁ" --> E["PATCH দিয়ে অবস্থা বদলান<br/>যেমন: PATCH /orders/12"]
    D -- "না, একটা প্রক্রিয়া চালু হয়" --> F["জিনিসের নিচে ক্রিয়া-পথ + POST<br/>যেমন: POST /books/7/publish"]
    C --> G{"জিনিসটা কি অন্য জিনিসের ভেতরে থাকে?<br/>রিভিউ, বইয়ের ভেতরে"}
    G -- "হ্যাঁ" --> H["Nested: /books/:bookId/reviews<br/>সর্বোচ্চ ৩ স্তর"]
    G -- "না" --> I["সরাসরি: /books"]
    H --> J["ফিল্টার/সাজানো/পেজ থাকলে Query-তে"]
    I --> J
    E --> J
    F --> J
```

### WordPress REST API-ও এই নিয়মই মানে

আপনি যেহেতু WordPress নিয়ে কাজ করেন, এই নিয়মগুলো নতুন কিছু নয় — WordPress নিজেই এগুলো মেনে চলে:

| WordPress Core | নিয়ম |
|---|---|
| `GET /wp-json/wp/v2/posts` | বহুবচন বিশেষ্য + Version (`v2`) |
| `GET /wp-json/wp/v2/posts/7` | Collection-এর ভেতরে এক-একটা |
| `GET /wp-json/wp/v2/posts?per_page=5&page=2` | পেজ/ফিল্টার Query-তে |
| `POST /wp-json/wp/v2/posts/7` | আপডেট (WordPress-এ POST-ও আপডেটের কাজ করে — এটা তাদের নিজস্ব সিদ্ধান্ত) |
| `GET /wp-json/wp/v2/users/me` | `me` নামের স্থির path |

নিজের প্লাগিনে `register_rest_route( 'myplugin/v1', '/books', ... )` লিখলে সেটাও একই ছাঁচ: `myplugin/v1` = namespace + version, `/books` = বহুবচন বিশেষ্য। শুধু Express-এ যেটা `router.get(path, handler)`, WordPress-এ সেটা `methods` + `callback` + `permission_callback`। ধারণা এক, লেখার ধরন আলাদা (১ নম্বর সেকশনের তুলনা-ছক দেখুন)।

---

## ২১. Route টেস্ট করার উপায় — ব্রাউজার যথেষ্ট নয়

### গল্প

মল বানানো শেষ। কিন্তু উদ্বোধনের আগে কেউ একজন ক্রেতা সেজে প্রতিটা দোকানে ঢুকে দেখবে — বোর্ডে যা লেখা, বাস্তবে কি তাই হচ্ছে? সদস্য কার্ড ছাড়া VIP তলায় ঢোকা যায় কিনা? ভুল ঠিকানায় গেলে ভদ্র উত্তর পাওয়া যায় কিনা? এই "ক্রেতা সেজে পরীক্ষা"-ই হলো **Route টেস্ট**।

মনে করিয়ে দিই — Address Bar থেকে শুধু **GET** যায়। POST, PATCH, DELETE ব্রাউজার থেকে পাঠাতে গেলে ফর্ম বা `fetch` লিখতে হয়। তাই আলাদা টুল লাগে।

### টুলের তালিকা

| টুল | কেমন | কখন ভালো |
|---|---|---|
| **ব্রাউজার (Address Bar)** | শুধু GET | `/api/v1/health` দেখে নেওয়া |
| **`curl`** | টার্মিনালের কমান্ড | দ্রুত এক-দুটো টেস্ট, README-তে উদাহরণ দেওয়া |
| **Postman / Thunder Client** | GUI অ্যাপ (Thunder Client = VS Code-এর ভেতরে) | প্রতিদিনের ডেভেলপমেন্ট, token রাখা, request সাজিয়ে সংরক্ষণ |
| **VS Code REST Client (`.http` ফাইল)** | কোডের পাশেই টেক্সট ফাইলে request | request গুলো Git-এ রেখে দলের সবাইকে দেওয়া |
| **`supertest` + `node --test`** | কোড দিয়ে স্বয়ংক্রিয় টেস্ট | একবার লিখে বারবার চালানো — কিছু ভাঙলে সাথে সাথে ধরা পড়ে |

### `curl` দিয়ে — BookNest-এর পুরো যাত্রা

সার্ভার চালু আছে ধরে নিন (`npm run dev`)। Git Bash / Linux / macOS টার্মিনালে:

```bash
BASE=http://localhost:5000/api/v1
# BASE = বারবার লিখতে না হয় তাই একটা শেল ভেরিয়েবলে ঠিকানার শুরুটা রাখলাম

# ১. স্বাস্থ্য পরীক্ষা
curl $BASE/health

# ২. সদস্য হওয়া (POST + JSON body)
curl -X POST $BASE/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"তিথি","email":"tithi@example.com","password":"secret123"}'
# -X POST = Method বলা, -H = header, -d = body
# উত্তরে token আসবে। নামে বাংলা আছে, তাই terminal-এর encoding UTF-8 হওয়া চাই

# ৩. লগইন করে token একটা ভেরিয়েবলে রাখা
TOKEN=$(curl -s -X POST $BASE/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"tithi@example.com","password":"secret123"}' \
  | node -pe 'JSON.parse(require("fs").readFileSync(0,"utf8")).token')
# -s = চুপচাপ (progress bar দেখাবে না)। শেষে node দিয়ে উত্তরের JSON থেকে শুধু token বের করলাম — আলাদা কিছু ইনস্টল লাগে না

# ৪. token দিয়ে নিজের পরিচয় দেখা
curl $BASE/auth/me -H "Authorization: Bearer $TOKEN"

# ৫. নতুন বই জমা (লগইন লাগে)
curl -X POST $BASE/books \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"গীতাঞ্জলি","author":"রবীন্দ্রনাথ ঠাকুর","price":250,"category":"poetry"}'
# উত্তরে বইয়ের _id আসবে — সেটা পরের ধাপগুলোতে লাগবে

# ৬. তালিকা — Query সহ
curl "$BASE/books?category=poetry&sort=price-low&page=1&limit=5"
# URL-এ & থাকলে অবশ্যই "" দিয়ে ঘিরবেন, নাহলে শেল & দেখে কমান্ড ভেঙে দেয়

# ৭. একটা বই ও আংশিক আপডেট (<id> জায়গায় ৫ নম্বর ধাপের _id বসান)
curl $BASE/books/<id>
curl -X PATCH $BASE/books/<id> \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"price":300}'

# ৮. মোছা — সাধারণ সদস্য হিসেবে 403 আসাই ঠিক (শুধু admin পারে)
curl -i -X DELETE $BASE/books/<id> -H "Authorization: Bearer $TOKEN"
# -i = উত্তরের status line আর header-ও দেখাও
```

> **Windows ব্যবহারকারীদের জন্য:** PowerShell-এ `curl` আসলে `Invoke-WebRequest`-এর ছদ্মনাম, আলাদাভাবে আচরণ করে। হয় **Git Bash** ব্যবহার করুন, নয়তো PowerShell-এ `curl.exe` লিখুন (তখনও JSON-এর কোটেশন নিয়ে ঝামেলা হয়)। বেশি ঝামেলা মনে হলে Postman বা `.http` ফাইলই সহজ।

**Admin হবেন কীভাবে?** Register-এ role নেওয়া হয় না (নিরাপত্তার কারণে — ১৫ নম্বর সেকশন)। তাই টেস্টের জন্য MongoDB-তে সরাসরি বদলে নিতে হবে। `mongosh` দিয়ে:

```javascript
use booknest
db.users.updateOne({ email: "tithi@example.com" }, { $set: { role: "admin" } })
// এরপর আবার লগইন করে নতুন token নিন। এখন DELETE /books/:id চলবে
```

### Postman-এ — token একবার সেট, সব জায়গায় ব্যবহার

1. **Environment** বানান, দুটো ভেরিয়েবল: `baseUrl` = `http://localhost:5000/api/v1`, আর `token` (খালি রাখুন)।
2. প্রতিটা request-এর URL হবে `{{baseUrl}}/books`।
3. **Login** request-এর **Tests** ট্যাবে এই এক লাইন লিখুন — লগইন সফল হলেই token নিজে থেকে সেভ হবে:

```javascript
pm.environment.set("token", pm.response.json().token);
// Postman-এর নিজস্ব স্ক্রিপ্ট: উত্তরের JSON পড়ে "token" Environment-এ রেখে দিলো
```

4. যে request-এ লগইন লাগে, তার **Authorization** ট্যাবে Type = **Bearer Token**, মান = `{{token}}`।

Thunder Client-এও একই ধারণা (Env আর Auth ট্যাব) — শুধু নামগুলো আলাদা।

### `.http` ফাইল — VS Code REST Client এক্সটেনশন

প্রজেক্টে `requests.http` নামে একটা ফাইল রাখুন। প্রতিটা request-এর ওপরে **Send Request** লেখা ভেসে উঠবে, ক্লিক করলেই চলবে:

```http
@baseUrl = http://localhost:5000/api/v1

### সদস্যের লগইন — নাম দিলাম "login", যাতে পরে এর উত্তর ব্যবহার করা যায়
# @name login
POST {{baseUrl}}/auth/login
Content-Type: application/json

{
  "email": "tithi@example.com",
  "password": "secret123"
}

### token ভেরিয়েবলে রাখা — login request-এর উত্তরের body থেকে
@token = {{login.response.body.token}}

### নিজের পরিচয়
GET {{baseUrl}}/auth/me
Authorization: Bearer {{token}}

### বইয়ের তালিকা
GET {{baseUrl}}/books?category=poetry&limit=5
```

`###` দিয়ে এক request থেকে আরেকটা আলাদা হয়। এই ফাইলটা Git-এ রাখলে দলের সবাই একই request পাবে — Postman-এর কালেকশন শেয়ার করার ঝামেলা নেই।

### স্বয়ংক্রিয় টেস্ট — `supertest` + `node --test`

হাতে হাতে Postman-এ ২০টা request চালানো ক্লান্তিকর, আর কোড বদলালে আবার সব চালাতে হয়। স্বয়ংক্রিয় টেস্ট এই কাজটা মেশিনকে দিয়ে দেয়। মনে আছে, **`app.js` আর `server.js` আলাদা রেখেছিলাম** (১৩ নম্বর সেকশন)? এবার তার ফল পাচ্ছি — টেস্টে শুধু `app` আনলেই হয়, port খোলা বা DB সংযোগ লাগে না।

```bash
npm install --save-dev supertest
# --save-dev = শুধু ডেভেলপমেন্টে লাগবে, production-এ নয়
```

`package.json`-এর `scripts`-এ যোগ করুন:

```json
"test": "node --test"
```

`node --test` Node.js-এর নিজস্ব টেস্ট রানার (Node 18+) — Jest বা Mocha ইনস্টল না করেই চলে। এটা `tests/` ফোল্ডার বা `*.test.js` নামের ফাইল নিজে খুঁজে নেয়।

**`tests/routes.test.js`**

```javascript
// চালানো: npm test
// এই টেস্টগুলো MongoDB ছাড়াই চলে — কারণ এগুলো এমন Route/Middleware পরীক্ষা করে, যা DB-তে পৌঁছানোর আগেই থেমে যায়
process.env.JWT_SECRET = "test-secret";
// টেস্টের জন্য নকল চাবি — আসল .env-এর দরকার নেই
process.env.NODE_ENV = "test";
process.env.CLIENT_URL = "http://localhost:5173";

const { test } = require("node:test");
// node:test = Node.js-এর নিজস্ব টেস্ট রানার (Node 18+), আলাদা কিছু ইনস্টল করা লাগে না
const assert = require("node:assert/strict");
const request = require("supertest");
// supertest = app-কে port না খুলেই HTTP request পাঠিয়ে উত্তর যাচাই করার টুল

const app = require("../src/app");
// এজন্যই app.js আর server.js আলাদা — এখানে শুধু app আনলাম, DB সংযোগ বা listen হলো না

test("GET /health → 200 আর status ok", async () => {
  const res = await request(app).get("/api/v1/health");
  assert.equal(res.status, 200);
  assert.equal(res.body.status, "ok");
});

test("অচেনা route → 404 JSON", async () => {
  const res = await request(app).get("/api/v1/abcd");
  assert.equal(res.status, 404);
  assert.equal(res.body.success, false);
});

test("token ছাড়া POST /books → 401", async () => {
  const res = await request(app).post("/api/v1/books").send({ title: "x" });
  assert.equal(res.status, 401);
});

test("ভুল token → 401", async () => {
  const res = await request(app)
    .get("/api/v1/auth/me")
    .set("Authorization", "Bearer this.is.not-a-real-token");
  assert.equal(res.status, 401);
});

test("ভুল id (abc) → 400, DB পর্যন্ত যায়ই না", async () => {
  const res = await request(app).get("/api/v1/books/abc");
  assert.equal(res.status, 400);
});

test("ভাঙা JSON → 400", async () => {
  const res = await request(app)
    .post("/api/v1/auth/login")
    .set("Content-Type", "application/json")
    .send('{ "email": ');
  assert.equal(res.status, 400);
});

test("NoSQL injection চেষ্টা (email হিসেবে object) → 400", async () => {
  const res = await request(app)
    .post("/api/v1/auth/login")
    .send({ email: { $ne: null }, password: { $ne: null } });
  assert.equal(res.status, 400);
});
```

`npm test` চালালে (এই ফাইলটা আমি সত্যিই চালিয়ে দেখেছি) আউটপুটে সাতটা টেস্টই `ok` আসে — এবং MongoDB চালু না থাকলেও।

**এই টেস্টগুলো DB ছাড়া চলে কেন?** কারণ এখানে যা যা পরীক্ষা করছি, সবই DB-তে পৌঁছানোর **আগেই** থেমে যায়:

| টেস্ট | কে থামায় | DB লাগে? |
|---|---|---|
| `/health` | সরাসরি উত্তর | না |
| অচেনা route → 404 | `notFound` | না |
| token ছাড়া → 401 | `protect`-এর প্রথম `if` | না |
| ভুল token → 401 | `jwt.verify` ব্যর্থ, DB দেখা হয়ইনি | না |
| ভুল id → 400 | `router.param` → `validateObjectId` | না |
| ভাঙা JSON → 400 | `express.json()` | না |
| Object দিয়ে NoSQL injection → 400 | controller-এর `typeof` পাহারা | না |

**যে টেস্টে আসল DB লাগে** (সঠিক password-এ লগইন, বই তৈরি করে তারপর মোছা) — সেগুলোর জন্য দুটো প্রচলিত উপায় আছে: একটা আলাদা **টেস্ট-ডেটাবেস** (`booknest_test`) ব্যবহার করে প্রতিবার আগে মুছে ফেলা, অথবা `mongodb-memory-server` প্যাকেজ (মেমরিতে অস্থায়ী MongoDB চালায়)। এই ফাইলে ওগুলো দেখাইনি, কারণ আমার পরীক্ষার পরিবেশে MongoDB চালানোর সুযোগ ছিল না — আর যা নিজে চালিয়ে দেখিনি, তা "কাজ করে" বলে চালাতে চাই না। কিন্তু ছাঁচটা ওপরের টেস্টের মতোই: `request(app).post(...).send(...)` তারপর `assert`।

**`supertest`-এ কীভাবে মনে রাখবেন:**

```javascript
await request(app)              // app-কে দাও (listen লাগে না)
  .post("/api/v1/books")        // Method + পথ
  .set("Authorization", "Bearer " + token)   // header বসাও
  .send({ title: "x" });        // JSON body পাঠাও
// ফেরত আসে res — res.status আর res.body দিয়ে যাচাই
```

> **একটা ছোট ফাঁদ যা আমি নিজে টেস্ট লিখতে গিয়ে খেয়েছি:** HTTP header-এ বাংলা (বা যেকোনো non-ASCII) অক্ষর দেওয়া যায় না — `set("Authorization", "Bearer বাংলা...")` লিখলে `Invalid character in header content` error আসে। Header-এর মান সবসময় ইংরেজি অক্ষর ও সংখ্যায়। বাংলা যাবে শুধু body-তে।

### প্রতিটা Route-এর জন্য টেস্টের চেকলিস্ট

একটা Route লিখলে অন্তত এই ছয়টা পরিস্থিতি একবার করে চালিয়ে দেখুন:

| # | পরিস্থিতি | কী আশা করবেন |
|---|---|---|
| ১ | ✅ সব ঠিক আছে | 200 / 201 / 204 |
| ২ | 🔒 token নেই | 401 |
| ৩ | 🚫 token আছে কিন্তু role ভুল (সাধারণ সদস্য → admin route) | 403 |
| ৪ | 🆔 id-র ছাঁচ ভুল (`abc`) | 400 |
| ৫ | 🔍 id-র ছাঁচ ঠিক, কিন্তু সেই জিনিস নেই | 404 |
| ৬ | 📝 body ভুল বা অসম্পূর্ণ (নাম নেই, price = `"abc"`) | 400 |

### ব্যর্থ হলে — Status Code দেখে কোথায় খুঁজবেন

Route কাজ না করলে সবার আগে **status code** দেখুন। ওটাই বলে দেয় কোথায় তাকাতে হবে:

```mermaid
flowchart TD
    S["Request পাঠালাম — কী status এলো?"] --> A{"Status"}
    A -- "404" --> B["Route মেলেনি বা জিনিস নেই<br/>👉 URL-এর বানান, Method,<br/>/api/v1 prefix, Route-এর ক্রম দেখুন<br/>👉 বার্তায় 'এই route আমাদের সার্ভারে নেই' থাকলে Route-ই মেলেনি"]
    A -- "400" --> C["ইনপুট ভুল<br/>👉 body-র ফিল্ড, id-র ছাঁচ, JSON ফরম্যাট,<br/>Content-Type: application/json দেখুন"]
    A -- "401" --> D["পরিচয় নেই<br/>👉 Authorization header আছে?<br/>'Bearer ' এর পরে token? মেয়াদ শেষ?"]
    A -- "403" --> E["পরিচয় আছে, অনুমতি নেই<br/>👉 user-এর role কী?<br/>বইয়ের মালিক কে?"]
    A -- "409" --> F["Duplicate<br/>👉 একই email বা একই বইয়ে দ্বিতীয় রিভিউ"]
    A -- "500" --> G["আমাদের কোডে বিপদ<br/>👉 সার্ভারের টার্মিনাল দেখুন —<br/>errorHandler '💥 অপ্রত্যাশিত error' ছাপে"]
    A -- "কোনো উত্তরই নেই / ঘুরছে" --> H["Handler-এ res বা next নেই<br/>👉 ৮ নম্বর সেকশনের 'hung request'"]
    A -- "CORS error (Console-এ লাল)" --> I["ব্রাউজার আটকেছে<br/>👉 ১৯ নম্বর সেকশন,<br/>CLIENT_URL মিলিয়ে দেখুন"]
```

> **টার্মিনাল আর ব্রাউজার — দুই জায়গাই দেখুন।** ক্লায়েন্ট শুধু status আর বার্তা পায়, কিন্তু আসল কারণ (`console.error`) থাকে সার্ভারের টার্মিনালে। আবার CORS error-এর ক্ষেত্রে উল্টো — সার্ভারে কিছুই দেখা যায় না, লাল লেখা থাকে শুধু **ব্রাউজারের Console-এ**।

---

## ২২. সাধারণ ভুল ও সমাধান — Route নিয়ে যেখানে সবাই হোঁচট খায়

### গল্প

প্রতিটা নতুন কর্মী মলে প্রথম মাসে প্রায় একই ভুলগুলো করে: ভুল তলায় চলে যায়, বোর্ডের ক্রম না বুঝে ভুল দোকানে পৌঁছায়, কাজ শেষ করে ক্রেতাকে জানাতে ভুলে যায়। অভিজ্ঞ ম্যানেজার এগুলো দেখেই বলে দিতে পারেন কী গোলমাল। নিচের তালিকাটা সেই অভিজ্ঞ ম্যানেজারের চোখ — **লক্ষণ দেখে কারণ চেনার** ছক। এই তালিকার error বার্তাগুলো আমি নিজের চালানো পরীক্ষা থেকে নিয়েছি, আন্দাজে লিখিনি।

### ক) Route মেলা নিয়ে

| লক্ষণ | সম্ভাব্য কারণ | সমাধান |
|---|---|---|
| `Cannot GET /xyz` (HTML পাতা) বা আমাদের JSON 404 | path, Method বা prefix মেলেনি | URL-এর বানান, Method আর `/api/v1` prefix মিলিয়ে দেখুন। মনে রাখুন Address Bar সবসময় GET পাঠায় |
| `/api/v1/books` দিলে 404, অথচ `/api/v1/books/books` কাজ করে | Router ফাইলে path আবার `/books` লিখেছেন, অথচ `router.use("/books", ...)`-এ prefix আগেই বসেছে | Router-এর ভেতরে prefix বাদ দিয়ে শুধু `"/"` আর `"/:id"` লিখুন (১০ নম্বর সেকশন) |
| `/users/me`-তে "me"-কে `id` হিসেবে ধরছে (400/404) | `/:id` আগে লেখা, `/me` পরে | **স্থির path আগে, `:parameter` পরে** (৮ নম্বর সেকশন) |
| সার্ভার চালু হওয়ার সময়ই `Missing parameter name at index 1: *` | Express 4-এর `app.get("*")` লিখেছেন | Express 5-এ `"/{*splat}"` (বা নাম দিয়ে `"/files/*splat"`) |
| `:id(\d+)` লিখতেই সার্ভার চালু হচ্ছে না | Express 5-এ path-এর ভেতরে regex সরানো হয়েছে | `router.param` বা `validateObjectId`-এর মতো middleware দিয়ে যাচাই করুন |
| `"/books/:id?"` কাজ করছে না | ঐচ্ছিক অংশের পুরোনো লেখা | Express 5-এ `"/books{/:id}"` |
| `req.params.id` দিয়ে খুঁজে পাচ্ছি না (`===` মিলছে না) | `req.params`-এর মান **সবসময় String** — array-র `id` যদি সংখ্যা হয়, `"5" === 5` মেলে না | `Number(req.params.id)` করুন |

### খ) Handler আর Response নিয়ে

| লক্ষণ | সম্ভাব্য কারণ | সমাধান |
|---|---|---|
| Postman শুধু ঘুরতেই থাকে, উত্তর আসে না | Handler-এ `res.xxx()` নেই, আর `next()`-ও নেই — কোনো একটা `if` শাখায় ভুলে গেছেন | **প্রতিটা শাখায়** হয় `res` দিয়ে উত্তর, নয় `next()` (৮ নম্বর সেকশন) |
| `Cannot set headers after they are sent to the client` | একই request-এ দুইবার উত্তর দিয়েছেন | `if`-এর ভেতরে `return res.status(...).json(...)` লিখুন |
| `req.body` হলো `undefined` | `express.json()` নেই, বা Route-এর **পরে** বসেছে, বা client-এ `Content-Type: application/json` নেই | `app.js`-এ Route-এর আগে বসান, Client-এ header দিন |
| `Cannot destructure property 'name' of 'req.body' as it is undefined` | ওপরের একই কারণ (Express 5-এ `{}` নয়, `undefined` আসে) | মূল কারণটা ঠিক করুন, চাইলে `req.body ?? {}` দিয়ে সাময়িক সামাল দিন |
| Error হলেও `200 OK` ফেরত যাচ্ছে | `res.json({ error: "..." })` লিখেছেন, status বসাননি | `res.status(400).json(...)` অথবা `throw new ApiError(400, ...)` |
| সব ঠিক, কিন্তু `req.query.category` কখনো String আবার কখনো array | `?category=a&category=b` — একই key দুইবার এলে array হয় | `[].concat(req.query.category ?? [])` |

### গ) Controller, Router আর Middleware নিয়ে

| লক্ষণ | সম্ভাব্য কারণ | সমাধান |
|---|---|---|
| সার্ভার চালুর সময় `Cannot read properties of undefined (reading 'json')` | `router.get("/", controller.getBooks())` — **শেষে `()` দিয়ে ফাংশনটা ডেকে ফেলেছেন**, `req`/`res` তখন নেই | `controller.getBooks` লিখুন, `()` ছাড়া (১১ নম্বর সেকশন) |
| `argument handler must be a function` | Handler-এর জায়গায় `undefined` গেছে — `require`-এর নাম বা `module.exports` মেলেনি, বা বানান ভুল | `console.log(controller)` করে দেখুন কোন নামগুলো আছে |
| Router-এর ভেতরে `req.params.bookId` হলো `undefined` | Nested Router-এ `mergeParams: true` নেই | `express.Router({ mergeParams: true })` (১০ নম্বর সেকশন) |
| Middleware বসালাম, কিন্তু কিছু হচ্ছে না | Route-এর **পরে** বসেছে | ক্রম দেখুন — Middleware আগে, Route পরে |
| সব Request-ই `404` দিচ্ছে, এমনকি সঠিকগুলোও | `notFound` Route-এর **আগে** বসিয়ে ফেলেছেন | `notFound` আর `errorHandler` সবসময় **একদম শেষে**, আর তাদের মধ্যে `notFound` আগে |
| Error Handler লিখেছি, কিন্তু চলছেই না (Express-এর ডিফল্ট error পাতা আসছে) | তিনটা parameter (`req, res, next`) দিয়েছেন | Error Handler-এ চারটা parameter **অবশ্যই**: `(err, req, res, next)` — `next` ব্যবহার না করলেও |
| `restrictTo("admin")` চালাতেই 500: `Cannot read properties of undefined (reading 'role')` | `restrictTo` বসেছে `protect`-এর **আগে**, তখনো `req.user` তৈরিই হয়নি | `protect` আগে, `restrictTo` পরে — যেমন `delete(protect, restrictTo("admin"), ...)` |
| `app.use("/books", handler)` অপ্রত্যাশিতভাবে `/books/7/x`-এও চলছে | `app.use` **prefix** ধরে মেলায়, `app.get` পুরো path মেলায় | নির্দিষ্ট Route-এর জন্য `app.get`/`router.get` (৯ নম্বর সেকশন) |

### ঘ) Auth আর নিরাপত্তা নিয়ে

| লক্ষণ | সম্ভাব্য কারণ | সমাধান |
|---|---|---|
| লগইন করতেই `secretOrPrivateKey must have a value` | `process.env.JWT_SECRET` খালি — `.env` নেই, নামে ভুল, বা `dotenv.config()` অন্য `require`-এর **পরে** চলেছে | `server.js`-এর একদম প্রথম লাইনে `require("dotenv").config()` আর `.env`-এর নাম মিলান |
| সব প্রটেক্টেড Route-এ `jwt malformed` (401) | `Bearer` ছাড়া শুধু token, বা token-টা ভুল জায়গা থেকে নেওয়া | `Authorization: Bearer <token>` — এক স্পেস দিয়ে |
| ওপরের মতোই, কিন্তু token-টা ঠিকই মনে হচ্ছে; বার্তা `invalid token` | token-এর ভেতরে `"` কোটেশন ঢুকে গেছে — `JSON.stringify(token)` করে `localStorage`-এ রেখেছেন | String হলে সরাসরি `localStorage.setItem("token", token)` |
| `401` আর `403` গুলিয়ে ফেলছি | দুটো আলাদা প্রশ্নের উত্তর | **401** = "আপনি কে, জানি না" (কার্ড নেই/ভুল)। **403** = "আপনাকে চিনি, কিন্তু এখানে ঢোকা মানা" (role/মালিকানা ভুল) |
| যে কেউ নিজেকে `admin` বানাতে পারছে | `User.create(req.body)` — পুরো body সরাসরি দিয়ে দিয়েছেন | Register-এ শুধু `name, email, password` বেছে নিন (**Mass Assignment**, ১৪-১৫ নম্বর সেকশন) |
| Login-এ `{ "email": { "$ne": null } }` দিলে ঢুকে যাচ্ছে | Query-তে সরাসরি বসানো — **NoSQL Injection** | `typeof email !== "string"` হলে 400 (আমরা ১৫ নম্বর সেকশনে করেছি) |

### ঙ) MongoDB আর Mongoose নিয়ে

| লক্ষণ | সম্ভাব্য কারণ | সমাধান |
|---|---|---|
| ভুল id দিলে 500 (`Cast to ObjectId failed`) | id-র ছাঁচ যাচাই হয়নি, Error Handler `CastError` চেনে না | `router.param("id", validateObjectId)` আর Error Handler-এ `CastError` → 400 |
| অনেকক্ষণ ঘুরে 500: `...buffering timed out after 10000ms` | MongoDB-তে সংযোগই হয়নি — `MONGO_URI` ভুল, `mongod` বন্ধ, বা Atlas-এ আপনার IP অনুমোদিত নেই | `MONGO_URI` মিলান, `mongod` চালু আছে কিনা দেখুন, Atlas-এর Network Access দেখুন |
| একই email-এ দ্বিতীয়বার register করলে অদ্ভুত error | MongoDB duplicate key (`code: 11000`) | Error Handler-এ `err.code === 11000` → **409** |
| Schema-র `min`/`enum` নিয়ম আপডেটের সময় কাজ করছে না | `findByIdAndUpdate` ডিফল্টে validator চালায় না | `findById` → ফিল্ড বসানো → `.save()` (আমরা `updateBook`-এ এটাই করেছি) |

### চ) React আর Deploy নিয়ে

| লক্ষণ | সম্ভাব্য কারণ | সমাধান |
|---|---|---|
| Console-এ লাল: `blocked by CORS policy` | Backend `cors()` বসায়নি, বা `CLIENT_URL` React-এর আসল ঠিকানার সাথে (port সহ!) মেলেনি | `app.use(cors({ origin: process.env.CLIENT_URL }))` — `localhost:5173` আর `127.0.0.1:5173` দুটো আলাদা Origin |
| Postman-এ চলছে, React-এ `Failed to fetch` | সার্ভার বন্ধ, ভুল port, নয়তো CORS | Network ট্যাবে request-টা দেখুন; Console-এ CORS লেখা আছে কিনা |
| Login-এর পরও `Authorization` header-সহ request আটকাচ্ছে (Preflight ব্যর্থ) | `cors()` বসেছে `protect`-এর পরে — `OPTIONS` request-এ তো token থাকে না | `cors()` **সবার আগে** (আমাদের `app.js`-এর মতো) |
| Deploy-এর পর `/books` পাতায় Refresh দিলেই 404 | সার্ভার `GET /books` চেনে না — ওটা React Router-এর ঠিকানা | `app.get("/{*splat}", ...)` দিয়ে `index.html` পাঠান, তার আগে `/api`-এর জন্য JSON 404 |
| সার্ভার চালুতে `EADDRINUSE: address already in use` | ওই port-এ আগের সার্ভার তখনো চলছে | পুরোনো টার্মিনালটা বন্ধ করুন, নয়তো `.env`-এ অন্য `PORT` দিন |

> **সবচেয়ে বড় পরামর্শ:** error বার্তাটা **পুরো পড়ুন**, কপি করে সার্চ করার আগে। এই তালিকার প্রায় সব error-এর বার্তাতেই কারণ লেখা থাকে — `Missing parameter name`, `argument handler must be a function`, `Cannot read properties of undefined (reading 'role')` — এগুলো ভয় দেখানোর জন্য নয়, ঠিকানা বলে দেওয়ার জন্য।

---

## ২৩. সারসংক্ষেপ ও Practice আইডিয়া

```mermaid
mindmap
  root((Express-এ Route))
    Route কী
      Method + Path + Handler
      Route Table = ডিরেক্টরি বোর্ড
      Frontend Route বনাম Backend Route
    Method
      GET - দেখা
      POST - তৈরি
      PUT - পুরো বদল
      PATCH - আংশিক বদল
      DELETE - মোছা
      Safe ও Idempotent
    Path
      Static
      Parameter - id
      Optional - Express 5-এ curly braces
      Wildcard - splat
      regex সরানো হয়েছে
    Request-এর চার পকেট
      req.params
      req.query
      req.headers
      req.body
    Response
      res.status ও res.json
      2xx সফল
      4xx Client-এর ভুল
      5xx Server-এর ভুল
      একই ছাঁচে উত্তর
    সাজানো
      next ও Route-এর ক্রম
      app.use বনাম app.get
      express.Router
      mergeParams ও router.param
      Controller আলাদা ফাইলে
    Middleware
      Application - Router - Route স্তর
      protect - JWT যাচাই
      restrictTo - Role যাচাই
      validateObjectId
    Error
      notFound - 404
      errorHandler - চার parameter
      Express 5-এ async error স্বয়ংক্রিয়
      ApiError
    REST নিয়ম
      বহুবচন বিশেষ্য
      URL-এ ক্রিয়া নয়
      Version - api v1
      Nesting সর্বোচ্চ ৩ স্তর
    React ও Deploy
      CORS ও Preflight
      Vite Proxy
      api.js একটাই জায়গা
      SPA fallback
    টেস্ট
      curl ও Postman
      REST Client
      supertest
```

### এক পাতায় Route-এর পুরো যাত্রা

```mermaid
flowchart LR
    R["📱 Request<br/>DELETE /api/v1/books/7"] --> C["🌐 cors"]
    C --> L["📝 logger"]
    L --> J["📦 express.json"]
    J --> V1["🏬 /api/v1 Router"]
    V1 --> V2["📚 /books Router"]
    V2 --> P["🆔 router.param<br/>validateObjectId"]
    P --> A["🔐 protect"]
    A --> RT["👑 restrictTo admin"]
    RT --> K["🧑‍💼 Controller<br/>deleteBook"]
    K --> DB[("🗄️ MongoDB")]
    K -- "204 No Content" --> OUT["📱 Response"]
    P -. "ভুল id" .-> E["🧾 errorHandler"]
    A -. "token নেই" .-> E
    RT -. "admin নয়" .-> E
    K -. "বই নেই" .-> E
    E -- "4xx / 5xx JSON" --> OUT
```

### গল্পের অভিধান — গল্পের কোনটা মানে কী

| গল্পে | টেকনিক্যাল নাম |
|---|---|
| স্বপ্ননগর মল | Express সার্ভার (`app`) |
| ক্রেতা তিথি | Client — React / Browser / Postman |
| ডিরেক্টরি বোর্ড | Route Table |
| বোর্ডের এক সারি (ঠিকানা + কাজ + কর্মী) | একটা Route |
| ঠিকানা: "৩ তলা / বইঘর" | Path |
| কাজ: "দেখবো / জমা দেবো / বদলাবো / ফেলবো" | HTTP Method (GET / POST / PUT-PATCH / DELETE) |
| "৭ নম্বর বই" — ঠিকানার ভেতরের নম্বর | Route Parameter (`:id`, `req.params`) |
| ঠিকানার শেষের চিরকুট: "সস্তা আগে, ২য় পাতা" | Query String (`req.query`) |
| ক্রেতার হাতের ফর্ম | Body (`req.body`) |
| খামের গায়ে লেবেল | Header (`req.headers`) |
| তলা ম্যানেজার | `express.Router()` |
| ম্যানেজারকে তলা বরাদ্দ করা | `app.use("/books", bookRouter)` |
| "৭ নম্বর বই" বলতেই আগে যাচাই | `router.param("id", ...)` |
| ওপরের তলার টোকেন নিচের তলা দেখতে পায় না | `mergeParams` না দিলে Parent Param হারানো |
| গেটের সিকিউরিটি | Application Middleware |
| VIP ঘরের দরজার প্রহরী | Route-level Middleware (`protect`, `restrictTo`) |
| "এগিয়ে যান" | `next()` |
| "এই ঘরে নয়, পরের ঘরে চেষ্টা করুন" | `next("route")` |
| বিক্রয়কর্মী | Controller |
| গুদাম | MongoDB + Mongoose Model |
| গুদামের তাকের নিয়ম (দাম ঋণাত্মক হবে না) | Mongoose Schema Validation |
| সদস্য কার্ড | JWT Token |
| কার্ডে সিল দেওয়ার গোপন চাবি | `JWT_SECRET` |
| "কার্ড দেখান" | `protect` |
| "এই তলায় শুধু ম্যানেজার" | `restrictTo("admin")` |
| "কার্ড নেই — আপনি কে জানি না" | 401 Unauthorized |
| "কার্ড আছে — কিন্তু এখানে ঢোকা মানা" | 403 Forbidden |
| "এই নামে কোনো দোকানই নেই" | 404 Not Found |
| অভিযোগ ডেস্ক | `errorHandler` (চার parameter) |
| "এই নামে দোকান নেই" ঘোষণাকারী | `notFound` |
| ইচ্ছাকৃত জানানো ভুল ("অনুমতি নেই") | `ApiError` |
| নিয়মমাফিক ঠিকানা লেখার সংস্কৃতি | REST |
| মলের নতুন সংস্করণ, পুরোনোটা চালু | `/api/v1`, `/api/v2` |
| অন্য মলের ক্রেতা ঢুকতে চাইলে গেটের অনুমতি | CORS |
| ঢোকার আগে "অনুমতি আছে?" জিজ্ঞেস | Preflight (`OPTIONS`) |
| ক্রেতার হাতের অ্যাপে "মলের ঠিকানা" এক জায়গায় লেখা | `api.js` / `BASE_URL` |
| মল খোলার আগে ক্রেতা সেজে পরীক্ষা | Route Testing (`curl`, Postman, `supertest`) |

### Practice-এর জন্য আইডিয়া

**হাতে-কলমে ভেঙে দেখা (ভুল থেকেই সবচেয়ে বেশি শেখা যায়):**

1. **ক্রম উল্টে দিন** — `book.routes.js`-এ `/latest` Route-কে `/:id`-এর **নিচে** নিয়ে যান। `GET /api/v1/books/latest` করলে কী আসে? কেন? তারপর ঠিক করে দিন।
2. **`mergeParams` তুলে দিন** — `review.routes.js` থেকে `{ mergeParams: true }` সরান। `GET /books/<id>/reviews`-এ কী ভাঙে?
3. **`protect` আর `restrictTo`-র ক্রম উল্টান** — `delete(restrictTo("admin"), protect, ...)` লিখে চালান। যে error আসে, সেটা এই ফাইলের ২২ নম্বর সেকশনে কোথায় আছে?
4. **`app.get("*", ...)` লিখুন** — সার্ভার চালু হতেই কী error? তারপর `/{*splat}` দিয়ে ঠিক করুন।
5. **Error Handler থেকে `next` parameter সরিয়ে দিন** — কী বদলে গেলো? Express কীভাবে বুঝতো এটা Error Handler?
6. **`try/catch` ছাড়া `async` Controller-এ ইচ্ছে করে `throw` করুন** — Express 5-এ error নিজে থেকেই Error Handler-এ পৌঁছায়। এবার আলাদা একটা ফোল্ডারে `npm install express@4` করে একই কোড চালান — কী ঘটে? (১৭ নম্বর সেকশনের `asyncHandler` তাহলে কেন লাগে, নিজের চোখে দেখুন)

**নতুন কিছু বানানো:**

7. **`PATCH /books/:id/stock`** নামে একটা ছোট Route বানান — শুধু `inStock` বদলাবে (`true/false`)। কোন Method? কোন Middleware? কে পারবে?
8. **`GET /auth/me`-এর মতো `PATCH /auth/me`** — নিজের `name` বদলানো। `email` আর `role` কেন বদলাতে দেওয়া যাবে না?
9. **`GET /books?author=...`** ফিল্টার যোগ করুন, তারপর `README`-তে Route Table-এ নতুন Query-টা লিখুন।
10. **Review-এ `PATCH`** — নিজের রিভিউ বদলানো (মালিক বা admin)। `deleteReview`-এর `bookId + reviewId` একসাথে মেলানোর পদ্ধতিটা নকল করুন।
11. **Rate limiting** — `express-rate-limit` ইনস্টল করে শুধু `/auth/login`-এ বসান (ব্রুট-ফোর্স ঠেকাতে)। কোন স্তরের Middleware এটা?
12. **`GET /api/v1/books/:id` দুটো আলাদা Router-এ** — `v1` আর `v2` নামে দুটো Router বানিয়ে `v2`-তে উত্তরের ছাঁচ একটু আলাদা করুন (১০ নম্বর সেকশনের দুই-জায়গায়-মাউন্ট কৌশল)।

**নিজের কাজের সাথে মেলানো:**

13. **নিজের কোনো WordPress প্লাগিনের `register_rest_route`-গুলো** একটা Route Table-এ লিখুন (Method | URL | কে ঢুকতে পারবে | সফল | ব্যর্থ)। তারপর ভাবুন — Express-এ সেটাকে কীভাবে সাজাতেন? `permission_callback` কোন Middleware-এর সমান?
14. **`supertest`-এ আরও তিনটা টেস্ট** যোগ করুন — DB লাগে না এমন (যেমন `POST /auth/register`-এ `password` না দিলে কী আসে? `PATCH /books/abc` কী দেয়?)।
15. **React-এর বইয়ের তালিকায় "নতুন বই যোগ" ফর্ম** বানান — `api.js`-এ `createBook` আছে, শুধু ফর্মটা জুড়তে হবে। ভুল হলে Backend-এর `message` ফর্মের নিচে দেখান।

---

## ২৪. প্রশ্ন ও উত্তর (Q&A)

> **কীভাবে ব্যবহার করবেন:** আগে প্রশ্নটা পড়ে নিজের মতো উত্তর ভাবুন, তারপর ▶ চাপ দিয়ে মিলিয়ে নিন। উত্তর না মিললে বন্ধনীতে দেওয়া সেকশনে ফিরে যান।

### ক) মূল ধারণা

<details>
<summary><b>প্রশ্ন ১.</b> Route কাকে বলে? তার উপাদান কী কী?</summary>

Route হলো Server-এর কাছে একটা **প্রতিশ্রুতি**: "**এই Method-এ, এই Path-এ Request এলে, এই Handler কাজটা করবে।**" তিনটা উপাদান: **HTTP Method** (GET/POST/…), **Path** (`/books/:id`), আর **Handler** (কাজের ফাংশন)।

```javascript
app.get("/books/:id", (req, res) => { res.json({ id: req.params.id }); });
//  ↑Method  ↑Path                   ↑Handler
```

সব Route মিলিয়ে হয় **Route Table** — মলের ডিরেক্টরি বোর্ড। *(সেকশন ১)*
</details>

<details>
<summary><b>প্রশ্ন ২.</b> Route আর Endpoint কি একই জিনিস?</summary>

দৈনন্দিন কথায় প্রায় একই অর্থে চলে। সূক্ষ্ম পার্থক্য দৃষ্টিভঙ্গিতে: **Route** হলো সার্ভারের দিক থেকে সংজ্ঞা (Method + Path + Handler)। **Endpoint** হলো Client-এর দিক থেকে "কোন ঠিকানায় ডাকবো" (`GET /api/v1/books`)। একজন লেখে, আরেকজন ডাকে।
</details>

<details>
<summary><b>প্রশ্ন ৩.</b> Frontend Route (React Router) আর Backend Route (Express)-এর পার্থক্য কী?</summary>

| | Frontend Route | Backend Route |
|---|---|---|
| কোথায় চলে | ব্রাউজারে, JavaScript দিয়ে | সার্ভারে |
| কাজ | কোন **পেজ/কম্পোনেন্ট** দেখাবে | কোন **কাজ** করবে, কোন **ডেটা** দেবে |
| উত্তর | HTML/UI (সার্ভারে যাওয়া লাগে না) | সাধারণত JSON |
| নেওয়া হয় | `/books` → `<BookList />` | `GET /api/v1/books` → JSON তালিকা |

একই ঠিকানা (`/books`) দুই জায়গায় দুই অর্থ হতে পারে, এজন্য Backend-এর সব Route `/api` prefix-এর নিচে রাখা হয়। নাহলে React-এর পেজে Refresh দিলে ব্রাউজার সরাসরি সার্ভারে `GET /books` পাঠিয়ে 404 পায়। *(সেকশন ২)*
</details>

<details>
<summary><b>প্রশ্ন ৪.</b> Route Handler, Controller আর Middleware — তিনটা কি আলাদা জিনিস?</summary>

তিনটাই আসলে **ফাংশন** — পার্থক্য ভূমিকায়:

- **Handler** = Route-এর শেষ ফাংশন, যে `res` দিয়ে উত্তর পাঠায়।
- **Controller** = Handler-কে আলাদা ফাইলে রাখার **সংগঠন-পদ্ধতি**। Controller-এর ফাংশনগুলোই Handler।
- **Middleware** = Handler-এর **আগে** বা মাঝখানে বসে পরীক্ষা/প্রস্তুতির কাজ করে, তারপর `next()` ডাকে (যেমন `protect`)।

মলের ভাষায়: Middleware = সিকিউরিটি, Controller/Handler = বিক্রয়কর্মী। *(সেকশন ৮, ১১, ১২)*
</details>

<details>
<summary><b>প্রশ্ন ৫.</b> আগে Route Table লিখতে বলা হয় কেন?</summary>

কারণ কাজের নকশা (কোন ঠিকানা, কোন Method, কার অনুমতি, কোন ব্যর্থতায় কোন status) কাগজে ঠিক না করে কোডে নামলে মাঝপথে বারবার ভাঙতে হয়। Route Table হলো **Frontend আর Backend দলের মধ্যে চুক্তিপত্রও** — Frontend ডেভেলপার Table দেখেই `api.js` লিখতে পারে, Backend শেষ হওয়ার অপেক্ষা না করে। *(সেকশন ১৩)*
</details>

### খ) Method, Path আর Request-এর তথ্য

<details>
<summary><b>প্রশ্ন ৬.</b> PUT আর PATCH-এর পার্থক্য কী?</summary>

**PUT** = পুরো জিনিসটা **প্রতিস্থাপন**। যে ফিল্ড পাঠালেন না, সেটা ডিফল্টে ফিরে যায় বা মুছে যায়। **PATCH** = শুধু যে ফিল্ডগুলো পাঠালেন **সেগুলোই** বদলায়।

উদাহরণ: বইয়ের `category` আছে `"poetry"`। `{ "price": 300 }` পাঠালে —

- **PATCH** → শুধু `price` বদলায়, `category` `"poetry"`-ই থাকে।
- **PUT** → `title`, `author`, `price` সব দিতে হয়; `category` না দিলে ডিফল্ট `"novel"`-এ ফিরে যায়।

বাস্তবে বেশিরভাগ ক্ষেত্রে ফর্মে একটা-দুটো ফিল্ড বদলালে **PATCH**-ই চান আপনি। *(সেকশন ৪)*
</details>

<details>
<summary><b>প্রশ্ন ৭.</b> "Safe" আর "Idempotent" বলতে কী বোঝায়? কেন জানা দরকার?</summary>

- **Safe** = সার্ভারের ডেটা **বদলায় না** (GET, HEAD)।
- **Idempotent** = একই Request **একবার বা দশবার** পাঠালে ফল **একই** (GET, PUT, DELETE)। POST idempotent নয় — দশবার POST করলে দশটা বই তৈরি হয়।

জানা দরকার কারণ: নেটওয়ার্ক ঝামেলা করলে Client/ব্রাউজার Request আবার পাঠাতে পারে। Idempotent Method হলে আবার পাঠানো নিরাপদ; POST হলে ডুপ্লিকেট তৈরি হতে পারে। এজন্যই **GET দিয়ে কখনো ডেটা বদলাতে নেই** — সার্চ ইঞ্জিন বা prefetch শুধু লিংক খুলেই সব মুছে দিতে পারে। *(সেকশন ৪, ২০)*
</details>

<details>
<summary><b>প্রশ্ন ৮.</b> <code>req.params</code>, <code>req.query</code> আর <code>req.body</code> — কোনটা কখন?</summary>

| কী | কোথায় থাকে | কখন ব্যবহার | উদাহরণ |
|---|---|---|---|
| `req.params` | Path-এর ভেতরে | **কোন জিনিস** — পরিচয় | `/books/7` → `id = "7"` |
| `req.query` | `?` এর পরে | **কীভাবে দেখাবো** — ফিল্টার/সাজানো/পেজ | `?category=novel&page=2` |
| `req.body` | Request-এর ভেতরের মাল | **নতুন/বদলানোর ডেটা** | `{ "title": "..." }` |
| `req.headers` | খামের লেবেল | **মেটা তথ্য** — token, ফরম্যাট | `Authorization: Bearer ...` |

সহজ সূত্র: **পরিচয় → params, ধরন → query, মাল → body, লেবেল → headers।** *(সেকশন ৬)*
</details>

<details>
<summary><b>প্রশ্ন ৯.</b> <code>req.params.id</code> দিয়ে <code>===</code> তুলনা মিলছে না কেন?</summary>

কারণ `req.params` আর `req.query`-এর **সব মান String**। `/books/5` হলে `req.params.id` হলো `"5"` (String), সংখ্যা `5` নয়। তাই `"5" === 5` হয় `false`।

```javascript
const id = Number(req.params.id);   // এখন সংখ্যা
```

MongoDB-র ObjectId-তে অবশ্য String-ই চলে (তবে আগে ছাঁচ যাচাই করে নেওয়া ভালো — `validateObjectId`)। *(সেকশন ৫, ৬)*
</details>

<details>
<summary><b>প্রশ্ন ১০.</b> Route-এর ক্রম কেন এত গুরুত্বপূর্ণ?</summary>

Express Route গুলো **ওপর থেকে নিচে** মেলায় এবং **প্রথম যেটা মেলে সেটাই চলে**, বাকিগুলো আর দেখে না।

```javascript
app.get("/users/:id", A);   // ← "/users/me" এসেও এখানে থামবে, id = "me"
app.get("/users/me",  B);   // ← কখনোই পৌঁছায় না
```

নিয়ম: **স্থির path আগে, `:parameter` পরে।** আমাদের `book.routes.js`-এ `/latest` তাই `/:id`-এর আগে। *(সেকশন ৮)*
</details>

<details>
<summary><b>প্রশ্ন ১১.</b> 200, 201 আর 204-এর মধ্যে কোনটা কখন?</summary>

- **200 OK** — সফল, উত্তরে ডেটা আছে (`GET`, `PATCH`, `PUT`)।
- **201 Created** — নতুন কিছু **তৈরি** হয়েছে (`POST /books`, `POST /auth/register`)।
- **204 No Content** — সফল, কিন্তু ফেরত দেওয়ার মতো কিছু নেই (`DELETE`)। **204-এ body পাঠাবেন না** — `res.status(204).end()`।

*(সেকশন ৭)*
</details>

<details>
<summary><b>প্রশ্ন ১২.</b> 401, 403 আর 404-এর মধ্যে তফাত কী?</summary>

- **401** — "আপনি কে, জানি না।" token নেই বা ভুল/মেয়াদোত্তীর্ণ।
- **403** — "আপনাকে চিনি, কিন্তু এটা করা মানা।" role বা মালিকানা ভুল।
- **404** — "এটা আমাদের কাছে নেই।" ঠিকানা বা জিনিস নেই।

একটা সূক্ষ্ম সিদ্ধান্ত: অন্যের বই বদলাতে গেলে আমরা **403** দিয়েছি ("বইটা আছে, কিন্তু আপনার নয়")। কিছু সিস্টেম জিনিসের অস্তিত্ব লুকাতে 404 দেয় — দুটোই চলে, শুধু সবখানে **একটা নিয়ম মানুন**। *(সেকশন ৭, ১৪)*
</details>

<details>
<summary><b>প্রশ্ন ১৩.</b> খালি তালিকা পেলে 404 দেবো, নাকি 200?</summary>

**200** আর `data: []`। 404 মানে "এই ঠিকানাই নেই" — কিন্তু `/books` ঠিকানা তো আছে, শুধু ভেতরে কোনো বই নেই। সম্পূর্ণ আলাদা কথা। উল্টোদিকে `GET /books/<সঠিক ছাঁচের কিন্তু নেই এমন id>` হলো **404**, কারণ নির্দিষ্ট জিনিসটাই নেই। *(সেকশন ২০)*
</details>

### গ) Router, Middleware আর Controller

<details>
<summary><b>প্রশ্ন ১৪.</b> <code>app.use()</code> আর <code>app.get()</code>-এর পার্থক্য কী?</summary>

| | `app.get("/books", h)` | `app.use("/books", h)` |
|---|---|---|
| Method | শুধু GET | **সব** Method |
| Path মেলানো | **পুরো** path মেলে | **শুরুর অংশ** (prefix) মেলে — `/books/7/x`-ও ধরে |
| Path-এর প্রিফিক্স | যেমন আছে | ভেতরে **কেটে বাদ** যায় (`req.url` ছোট হয়, `req.baseUrl`-এ থাকে) |
| সাধারণ কাজ | নির্দিষ্ট Route | Middleware আর Router বসানো |

*(সেকশন ৯)*
</details>

<details>
<summary><b>প্রশ্ন ১৫.</b> <code>next()</code>, <code>next("route")</code> আর <code>next(err)</code> — কোনটা কী করে?</summary>

- `next()` — "আমার কাজ শেষ, **পরের handler**-এ যাও।"
- `next("route")` — "এই Route-এর বাকি handler গুলো **বাদ দাও**, পরের **Route** দেখো।"
- `next("router")` — "এই **পুরো Router** ছেড়ে বেরিয়ে যাও।"
- `next(err)` — "বিপদ!" → সব সাধারণ handler **লাফ দিয়ে** সোজা **Error Handler**-এ।

মনে রাখুন: Handler-এ **হয় `res` দিয়ে উত্তর, নয় `next()`** — দুটোর একটাও না থাকলে Request ঝুলে থাকে। *(সেকশন ৮)*
</details>

<details>
<summary><b>প্রশ্ন ১৬.</b> <code>express.Router()</code> কেন লাগে? সব Route তো <code>app</code>-এই লেখা যায়।</summary>

যায়, ছোট প্রজেক্টে। কিন্তু Route ১০০টা হলে একটা ফাইলে সব লেখা অসম্ভব হয়ে ওঠে। `Router` = **মিনি-অ্যাপ** — নিজের Route, নিজের Middleware, নিজের `param` নিয়ে আলাদা ফাইলে থাকে। তারপর `app.use("/books", bookRouter)` দিয়ে একটা prefix-এর নিচে বসানো হয়। ভেতরের Route-এ prefix লিখতে হয় না, তাই একই Router `/v1/books` আর `/v2/books`-এ দুইবারও বসানো যায়। *(সেকশন ১০)*
</details>

<details>
<summary><b>প্রশ্ন ১৭.</b> Nested Router-এ <code>mergeParams: true</code> কী কাজ করে?</summary>

`/books/:bookId/reviews` — এখানে `:bookId` লেখা আছে **বইয়ের Router-এ**, কিন্তু handler চলে **রিভিউয়ের Router-এ**। ডিফল্টে ভেতরের Router শুধু **নিজের** path-এর parameter দেখে, ওপরেরটা নয়। ফলে `req.params.bookId` হয় `undefined`। `express.Router({ mergeParams: true })` দিলে ওপরের Router-এর parameter-ও ভেতরে দেখা যায়। *(সেকশন ১০, ১৬)*
</details>

<details>
<summary><b>প্রশ্ন ১৮.</b> <code>router.param()</code> কী? আমরা কেন ব্যবহার করেছি?</summary>

`router.param("id", fn)` মানে: "এই Router-এর যেকোনো Route-এ `:id` থাকলেই, Route-এর handler-এর **আগে** `fn` চালাও।" আমরা `validateObjectId` বসিয়েছি — যাতে `/books/abc` ধরনের ভুল id DB-তে পৌঁছানোর আগেই **400** পায়, আর প্রতিটা Route-এ আলাদা করে লিখতে না হয়। *(সেকশন ১০)*
</details>

<details>
<summary><b>প্রশ্ন ১৯.</b> Middleware তিন স্তরে বসে — কোন স্তরে কী রাখবো?</summary>

| স্তর | কীভাবে | কী রাখবেন |
|---|---|---|
| **Application** | `app.use(...)` | সবার জন্য: `cors`, `logger`, `express.json` |
| **Router** | `router.use(...)` | সেই Router-এর সব Route-এর জন্য (যেমন পুরো `/admin` এলাকায় `protect`) |
| **Route** | `router.delete("/:id", protect, restrictTo("admin"), handler)` | নির্দিষ্ট Route-এর জন্য |

যত ছোট পরিসর, তত নির্দিষ্ট। *(সেকশন ১২)*
</details>

<details>
<summary><b>প্রশ্ন ২০.</b> Controller আলাদা ফাইলে রাখি কেন? আর <code>controller.getBooks</code> ও <code>controller.getBooks()</code>-এর পার্থক্য?</summary>

**কেন:** Route ফাইল = "কোথায় কী" (বোর্ড), Controller ফাইল = "কীভাবে" (কর্মী)। দুটো আলাদা থাকলে বোর্ড এক নজরে পড়া যায়, আর কর্মীর কাজ আলাদাভাবে টেস্ট/বদল করা যায়।

**পার্থক্য:**

```javascript
router.get("/", controller.getBooks);    // ✅ ফাংশনটা "দিলাম" — Request এলে Express ডাকবে
router.get("/", controller.getBooks());  // ❌ এখনই "ডেকে ফেললাম" — এর ফল (undefined) Route-এ বসলো
```

দ্বিতীয়টায় সার্ভার চালুর সময়ই error (`Cannot read properties of undefined`), কারণ ডাকার সময় `req`/`res` নেই। *(সেকশন ১১, ২২)*
</details>

<details>
<summary><b>প্রশ্ন ২১.</b> Error Handler-এ চারটা parameter কেন লাগে? তিনটা দিলে কী হয়?</summary>

Express Middleware-এর **parameter-সংখ্যা গুনে** বুঝে নেয় সেটা Error Handler কিনা। চারটা (`err, req, res, next`) = Error Handler; তিনটা = সাধারণ Middleware। `next` ব্যবহার না করলেও চারটাই লিখতে হয়। তিনটা দিলে সেটা সাধারণ Middleware ধরা হবে, আর Error আসলে Express-এর ডিফল্ট error পাতা আসবে। Error Handler বসবে **সবার শেষে**। *(সেকশন ১৭)*
</details>

<details>
<summary><b>প্রশ্ন ২২.</b> <code>app.js</code> আর <code>server.js</code> আলাদা করি কেন?</summary>

`app.js` শুধু অ্যাপ বানায় ও সাজায়, port খোলে না। `server.js` DB জোড়ে আর port খোলে। ফলে **টেস্টে** শুধু `app` আনলেই হয় — `supertest` নিজেই অস্থায়ী port খুলে নেয়, DB সংযোগও লাগে না (যতক্ষণ Route DB পর্যন্ত না যায়)। *(সেকশন ১৩, ২১)*
</details>

### ঘ) Auth আর নিরাপত্তা

<details>
<summary><b>প্রশ্ন ২৩.</b> JWT কীভাবে কাজ করে? Payload কি গোপন থাকে?</summary>

JWT = তিন অংশ: `header.payload.signature`। লগইনে সার্ভার `{ id }` ভরে **গোপন চাবি (`JWT_SECRET`) দিয়ে সিল** করে token দেয়। পরে Client প্রতি Request-এ `Authorization: Bearer <token>` পাঠায়; সার্ভার সিল মিলিয়ে দেখে (`jwt.verify`) — বদলানো হয়নি, মেয়াদ শেষ হয়নি।

**⚠️ Payload গোপন নয়** — এটা শুধু Base64 করা, encrypt করা নয়; যে কেউ পড়তে পারে। তাই payload-এ password বা গোপন কিছু রাখবেন না। সিল থাকে বলে **বদলানো** যায় না, কিন্তু **পড়া** যায়। *(সেকশন ১২)*
</details>

<details>
<summary><b>প্রশ্ন ২৪.</b> <code>protect</code> আর <code>restrictTo</code>-র কাজ কী? কেন এই ক্রমে?</summary>

- **`protect`** = "আপনি কে?" — token যাচাই করে, DB থেকে user এনে `req.user`-এ বসায়। ব্যর্থ হলে **401**।
- **`restrictTo("admin")`** = "আপনি কি অনুমোদিত?" — `req.user.role` দেখে। ব্যর্থ হলে **403**।

`restrictTo` **`req.user`-এর ওপর নির্ভর করে**, যেটা বানায় `protect`। তাই `protect` আগে না বসালে `req.user` `undefined` থাকে আর `restrictTo` 500 error দেয়। আগে "আপনি কে", তারপর "আপনি কি পারবেন"। *(সেকশন ১২)*
</details>

<details>
<summary><b>প্রশ্ন ২৫.</b> Mass Assignment আর NoSQL Injection কী? আমরা কীভাবে ঠেকিয়েছি?</summary>

**Mass Assignment:** `Book.create(req.body)` লিখলে ক্রেতা body-তে `"createdBy": "অন্যের id"` বা `"role": "admin"` ঢুকিয়ে দিতে পারে। **ঠেকানো:** body থেকে **বেছে বেছে** শুধু অনুমোদিত ফিল্ড নেওয়া (`title, author, price, ...`) আর `createdBy` বসানো `req.user` থেকে।

**NoSQL Injection:** লগইনে `{ "email": { "$ne": null }, "password": { "$ne": null } }` পাঠালে সরাসরি query-তে বসালে "কোনো একজন user"-কে পেয়ে যেতে পারে। **ঠেকানো:** `typeof email !== "string"` হলে 400 — String ছাড়া কিছু ঢুকতেই দিই না। *(সেকশন ১৪, ১৫)*
</details>

<details>
<summary><b>প্রশ্ন ২৬.</b> Token কোথায় রাখবো — <code>localStorage</code> নাকি cookie?</summary>

সৎ উত্তর: **দুটোরই দুর্বলতা আছে, আর এটা আলাদা বড় একটা বিষয়।** এই ফাইলে সহজ রাখতে `localStorage` ব্যবহার করেছি। সংক্ষেপে:

- `localStorage`: সহজ, কিন্তু কোনোভাবে XSS (ক্ষতিকর JS) ঢুকলে token চুরি যেতে পারে।
- `httpOnly` cookie: JS পড়তে পারে না (XSS-এ নিরাপদ), কিন্তু CSRF-এর কথা ভাবতে হয় আর CORS-এ `credentials` সেট করতে হয়।

বাস্তব প্রজেক্টে সিদ্ধান্ত নেওয়ার আগে এই বিষয়টা আলাদা করে পড়ে নিন। *(সেকশন ১৫)*
</details>

<details>
<summary><b>প্রশ্ন ২৭.</b> Login-এ "email ভুল" আর "password ভুল" আলাদা বার্তা দিই না কেন?</summary>

আলাদা বললে হ্যাকার জেনে যায় কোন email নিবন্ধিত আছে। তাই দুই ক্ষেত্রেই এক বার্তা: **"email বা password ভুল।"** *(সেকশন ১৫)*
</details>

### ঙ) Express 4 বনাম Express 5

<details>
<summary><b>প্রশ্ন ২৮.</b> Route-সংক্রান্ত Express 4 → 5 পরিবর্তনগুলো কী কী?</summary>

| | Express 4 | Express 5 |
|---|---|---|
| সব-ধরা | `"*"` | `"/{*splat}"` (নাম দিতে হয়) |
| ঐচ্ছিক অংশ | `"/books/:id?"` | `"/books{/:id}"` |
| Path-এ regex | `"/:id(\\d+)"` | সরানো হয়েছে |
| `async` handler-এর error | নিজে ধরতে হয় | নিজে থেকেই Error Handler-এ যায় |
| `req.body` (parser ছাড়া) | `{}` | `undefined` |
| `app.del()`, `req.param()` | ছিল | সরানো |

পুরনো টিউটোরিয়াল থেকে কপি করা Route ভাঙলে প্রথম সন্দেহ **Express-এর ভার্সন**। *(সেকশন ৩)*
</details>

<details>
<summary><b>প্রশ্ন ২৯.</b> Express 5-এ <code>app.get("*", ...)</code> লিখলে কী হয়? React-এর SPA fallback কীভাবে লিখবো?</summary>

সার্ভার **চালু হওয়ার সময়ই** error দেয় (`Missing parameter name at index 1: *`)। কারণ Express 5-এ wildcard-এর একটা **নাম** থাকতে হয়। SPA fallback:

```javascript
app.get("/{*splat}", (req, res) => {
  res.sendFile(path.join(clientBuild, "index.html"));
});
```

`/{*splat}` root (`/`) সহ সব GET ধরে। *(সেকশন ৩, ১৯)*
</details>

### চ) React ও Deploy

<details>
<summary><b>প্রশ্ন ৩০.</b> CORS কী? কেন আসে? কীভাবে ঠিক করবো?</summary>

ব্রাউজারের নিরাপত্তা নিয়ম (**Same-Origin Policy**): এক **Origin** (protocol + domain + port) থেকে লোড হওয়া পেজ অন্য Origin-এর সার্ভারের উত্তর পড়তে পারে না — যতক্ষণ না সেই সার্ভার অনুমতি দেয়। React (`localhost:5173`) আর Express (`localhost:5000`) — port আলাদা, তাই আলাদা Origin।

**সমাধান:** Backend-এ `cors` middleware (`origin` = React-এর ঠিকানা), অথবা Development-এ Vite Proxy। **মনে রাখুন:** CORS আটকায় **ব্রাউজার**, সার্ভার নয় — তাই Postman-এ চলে, ব্রাউজারে চলে না। *(সেকশন ১৯)*
</details>

<details>
<summary><b>প্রশ্ন ৩১.</b> Preflight (<code>OPTIONS</code>) request কী?</summary>

`Authorization` header বা JSON `Content-Type`-এর মতো "অ-সাধারণ" Request পাঠানোর **আগে** ব্রাউজার নিজে থেকে একটা `OPTIONS` Request পাঠিয়ে জিজ্ঞেস করে: "এই Origin থেকে এই Method-এ এই header সহ পাঠালে চলবে?" সার্ভার হ্যাঁ বললে আসল Request যায়। এজন্য `cors()` অবশ্যই **`protect`-এর আগে** বসাতে হয় — `OPTIONS`-এ token থাকে না। *(সেকশন ১৯)*
</details>

<details>
<summary><b>প্রশ্ন ৩২.</b> Deploy করার পর <code>/books</code> পেজে Refresh দিলে 404 কেন?</summary>

`/books` React Router-এর ঠিকানা, Express-এর নয়। Refresh দিলে ব্রাউজার সরাসরি সার্ভারে `GET /books` পাঠায় — সার্ভার চেনে না। সমাধান: `/api` দিয়ে শুরু হওয়া অমিল Request-এ JSON 404, আর **বাকি সব GET-এ `index.html`** পাঠানো (প্রশ্ন ২৯-এর কোড)। *(সেকশন ২, ১৯)*
</details>

### ছ) WordPress থেকে এলে

<details>
<summary><b>প্রশ্ন ৩৩.</b> WordPress-এর <code>register_rest_route</code>-এর অংশগুলো Express-এ কোনটার সমান?</summary>

| WordPress | Express |
|---|---|
| `'myplugin/v1'` (namespace + version) | `app.use("/api/v1", ...)` |
| `'/books/(?P<id>\d+)'` (route + regex) | `"/books/:id"` (Express 5-এ regex নেই — `router.param`/validation দিয়ে যাচাই) |
| `'methods' => 'GET'` | `router.get(...)` |
| `'callback'` | Controller / Handler |
| `'permission_callback'` | `protect` + `restrictTo(...)` Middleware |
| `'args'` (`validate_callback`, `sanitize_callback`) | ইনপুট যাচাই Middleware/Controller (যেমন `typeof` পাহারা, `validateObjectId`) |
| `WP_Error` ফেরত দেওয়া | `throw new ApiError(status, message)` |
| `WP_REST_Response` | `res.status(...).json(...)` |

ধারণা এক; **Express-এ প্রতিটা ধাপ আপনার নিজের হাতে সাজানো** — সেই স্বাধীনতাই আবার দায়িত্বও। *(সেকশন ১, ২০)*
</details>

### জ) টেস্ট

<details>
<summary><b>প্রশ্ন ৩৪.</b> Route টেস্ট করতে ব্রাউজার কেন যথেষ্ট নয়? কী ব্যবহার করবো?</summary>

Address Bar সবসময় **GET** পাঠায়; POST/PATCH/DELETE-এর জন্য ফর্ম বা `fetch` লাগে। দ্রুত হাতে-টেস্টে **Postman / Thunder Client / `curl` / REST Client**। বারবার চালানোর জন্য **`supertest` + `node --test`** — একবার লিখে রাখুন, কোড বদলালেই `npm test`। প্রতিটা Route-এর জন্য অন্তত ছয়টা পরিস্থিতি: সফল, token নেই (401), role ভুল (403), id-র ছাঁচ ভুল (400), জিনিস নেই (404), body ভুল (400)। *(সেকশন ২১)*
</details>

### ঝ) কোড পড়ে বলুন

নিচের প্রশ্নগুলোতে কোড দেখে নিজে ভাবুন **কী হবে**, তারপর উত্তর মিলিয়ে নিন।

<details>
<summary><b>কোড ১.</b> কোন handler চলবে?</summary>

```javascript
app.get("/users/:id", handlerA);
app.get("/users/me",  handlerB);
// GET /users/me
```

**`handlerA`** — `/users/:id` আগে লেখা, আর `"me"` তো একটা String, `:id`-তে মিলে যায়। প্রথম মিলটাই জেতে। `handlerB` কখনো চলবে না। ঠিক করতে দুটোর জায়গা বদলান।
</details>

<details>
<summary><b>কোড ২.</b> কী ছাপা হবে?</summary>

```javascript
app.get("/books/:id", (req, res) => {
  res.send(typeof req.params.id);
});
// GET /books/5
```

**`string`** — Route Parameter সবসময় String, `/books/5`-এর `5`-ও।
</details>

<details>
<summary><b>কোড ৩.</b> কোন কোন URL এই Route-এ মিলবে?</summary>

```javascript
app.use("/books", bookRouter);
// bookRouter-এর ভেতরে: router.get("/", listBooks);
```

**`GET /books`** (এবং শেষে স্ল্যাশ দিয়ে `GET /books/`)। `app.use("/books")`-এ prefix `/books` কেটে যায়, ভেতরের Router দেখে path হলো `/` — সেটা `router.get("/")`-এর সাথে মেলে। `/books/7` মিলবে না (ভেতরে `/:id` না থাকলে)।
</details>

<details>
<summary><b>কোড ৪.</b> কোন কোন URL মিলবে? (Express 5)</summary>

```javascript
app.get("/books{/:id}", handler);
```

**`/books`** আর **`/books/7`** — দুটোই। `{ }`-এর ভেতরের অংশটা ঐচ্ছিক। (Express 4-এ লেখা হতো `"/books/:id?"`)।
</details>

<details>
<summary><b>কোড ৫.</b> <code>req.params.bookId</code> কী পাবে?</summary>

```javascript
// book.routes.js
router.use("/:bookId/reviews", reviewRouter);

// review.routes.js
const router = express.Router();            // ← কোনো অপশন নেই
router.get("/", (req, res) => res.json({ bookId: req.params.bookId }));
// GET /books/64f1a2b3c4d5e6f7a8b9c0d1/reviews
```

**`undefined`** — `mergeParams: true` নেই, তাই ভেতরের Router ওপরের `:bookId` দেখতে পায় না। `express.Router({ mergeParams: true })` করলে আইডিটা পাওয়া যাবে।
</details>

<details>
<summary><b>কোড ৬.</b> Express 5-এ এই Request-এ কী উত্তর যাবে?</summary>

```javascript
app.get("/boom", async (req, res) => {
  throw new ApiError(403, "নিষেধ!");
});
app.use(errorHandler);
// GET /boom
```

**403** আর `{ "success": false, "message": "নিষেধ!" }`। Express 5 `async` handler-এর reject/throw নিজে থেকেই `next(err)`-এ পাঠায়, তাই কোনো `try/catch` লাগে না। (Express 4-এ এই error কেউ ধরতো না।)
</details>

---

## ✅ কোনটা কীভাবে যাচাই করেছি — একটা সৎ নোট

এই ফাইলের কোড শুধু লিখে ছেড়ে দিইনি; আলাদা একটা পরীক্ষার পরিবেশে (**Node.js v22, Express 5.2.1, Mongoose 8.24.4, jsonwebtoken 9.0.3, bcryptjs 3.0.3, supertest 7.2.2**) চালিয়ে দেখেছি। কোনটা কতদূর:

| বিষয় | অবস্থা |
|---|---|
| Route মেলা, ক্রম, `:param`, `{}`, `*splat`, regex-এর error, `Router`, `mergeParams`, `router.param`, `next("route")` ইত্যাদি | ✅ সরাসরি চালিয়ে দেখা, আউটপুট মিলিয়ে লেখা |
| Error বার্তাগুলো (`Missing parameter name…`, `argument handler must be a function`, `secretOrPrivateKey must have a value`, `jwt malformed` ইত্যাদি) | ✅ সরাসরি চালিয়ে পাওয়া |
| Controller/Middleware/Error Handler-এর আচরণ (401, 403, 400, 409, নেস্টেড CastError ইত্যাদি) | ✅ Model-এর জায়গায় নকল (stub) বসিয়ে আসল Controller ও Middleware চালানো হয়েছে |
| `supertest`-এর ৭টা টেস্ট (`npm test`) | ✅ সবগুলো `ok` |
| **আসল MongoDB-র সাথে পুরো end-to-end** (আসল register → login → বই তৈরি → মোছা) | ⚠️ **আমার পরীক্ষার পরিবেশে MongoDB চালানো যায়নি**, তাই আসল ডেটাবেসে এই ধাপ আমি নিজে চালাইনি |

তাই আপনি যখন প্রথমবার `npm run dev` করে আসল MongoDB-র সাথে চালাবেন, `curl`/Postman-এর সেই ধাপগুলো (২১ নম্বর সেকশন) একবার শুরু থেকে শেষ পর্যন্ত চালিয়ে নিন। কোথাও আটকালে ২২ নম্বর সেকশনের ছক আর error বার্তাটা মিলিয়ে দেখুন — Model-এর জায়গায় Mongoose-এর আসল আচরণ কিছু আলাদা হলে সেটা এখানেই ধরা পড়বে।

---

