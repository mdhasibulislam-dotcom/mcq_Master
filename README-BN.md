# MCQ Master v2

Admin panel থেকে প্রশ্ন ও mock exam বানাও; student panel-এ practice (সঙ্গে সঙ্গে উত্তর + ব্যাখ্যা), timed exam, result review, bookmark ও history।
Frontend: HTML/CSS/JS। Database ও login: Supabase।

## ১) Supabase setup (একবার)
1. Supabase → SQL Editor → New query → `sql/schema.sql` পুরোটা paste করে **Run**। বারবার run করা নিরাপদ।
2. Authentication → Providers → Email চালু আছে কিনা দেখো।
3. Authentication → URL Configuration:
   - **Site URL**: তোমার `https://YOUR-PROJECT.pages.dev`
   - **Redirect URLs**: `https://YOUR-PROJECT.pages.dev/reset.html` (Forgot password-এর জন্য)
4. টেস্টের সময় Authentication → Sign In / Providers → Email-এ "Confirm email" বন্ধ রাখতে পারো। Production-এ চালু রাখো।

## ২) নিজেকে Admin বানানো
1. সাইটে Create account দিয়ে account বানাও।
2. SQL Editor-এ (নিজের email বসিয়ে):
```sql
update public.profiles set role = 'admin' where email = 'YOUR_EMAIL@example.com';
```
3. Sign out করে আবার Login করো → Admin panel খুলবে।

## ৩) Cloudflare Pages-এ আপলোড
ZIP extract করো। `index.html` ফাইলটা extract করা ফোল্ডারের সরাসরি ভিতরে আছে। Workers & Pages → Create → Pages → Direct Upload → ফোল্ডারটা (অথবা ভিতরের সব ফাইল) আপলোড করো। `index.html` অবশ্যই root-এ থাকতে হবে, নাহলে 404 আসবে।

## ৪) এই version-এ যা ঠিক/নতুন হয়েছে
- Practice-এ instant correct/wrong + explanation; test শেষে score, subject-wise bar ও পুরো answer review (History থেকেও Review করা যায়)
- Exam-এ Previous/Next, প্রশ্ন-নম্বর navigator, উত্তরহীন প্রশ্নের সতর্কতা
- Forgot password (`reset.html`), duplicate username আগেই ধরা পড়ে
- Admin: exam Publish/Unpublish, প্রশ্ন-সংখ্যা দেখা, tag দেখা, filter ঠিক থাকে
- Schema: পুরনো টেবিলে নতুন column যোগ, আগের user-দের profile backfill, student অন্য user-এর email/phone দেখতে পায় না, student নিজের role/email/username বদলাতে পারে না
- ১০০০+ প্রশ্ন/result থাকলেও সব লোড হয়; সঠিক shuffle; modal desktop-এ ঠিক জায়গায়; timer ঠিক হয়; supabase-js version pin করা

## ৫) সীমাবদ্ধতা (সৎভাবে)
- Student-এর browser-এ সঠিক উত্তর পাঠানো হয় (instant feedback-এর জন্য), তাই কেউ চাইলে API দেখে উত্তর পেতে পারে।
- Score ও timer browser থেকে আসে, কেউ নকল result জমা দিতে পারে। Ranking/proctored exam চাইলে server-side grading (RPC) আলাদা করে বানাতে হবে।
- `service_role`/secret key কখনো এই ফোল্ডারে রাখবে না। `SUPABASE_JWKS_URL` এই frontend-এ লাগে না।
