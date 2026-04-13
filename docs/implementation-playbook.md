# Implementation Playbook

هذا الملف مو نظري.

هذا هو التشغيل الفعلي.

إذا بتطبق `Grounded LLM Wiki` عندك، فهذي هي الخطوات اللي تمشي عليها من أول يوم إلى الاستخدام اليومي.

## أولًا: شلون تجهز المشروع

### 1. سو مجلد للأصول

هذا هو `raw/`

كل شي فيه يعتبر evidence:

- مقالات
- PDFs
- transcripts
- ملاحظات أصلية
- بيانات

لا تعدلها.

### 2. سو مجلد للطبقة اللي يكتبها النموذج

هذا هو `wiki/`

وفيه كبداية:

- `index.md`
- `log.md`
- `conflicts.md`
- `sources/`
- `entities/`
- `concepts/`
- `syntheses/`
- `provisional/`

### 3. حط ملف schema مناسب لأداتك

استخدم واحد من هذي الأسماء:

- `AGENTS.md`
- `CODEX.md`
- `CLAUDE.md`
- `GEMINI.md`

المهم مو الاسم.

المهم إن القواعد تكون موجودة وواضحة.

## ثانيًا: شلون تدخل مصدر جديد

لا تبدأ كل ingest كأنه إعادة بناء للمشروع كله.

امش بهالترتيب:

1. أضف المصدر إلى `raw/`
2. اطلب من النموذج قراءة المصدر فقط
3. خل النموذج يسوي `source-note`
4. حدث `index.md`
5. إذا فعلاً فيه entity أو concept مهم، حدث صفحة وحدة أو صفحتين بالكثير
6. إذا ظهر تعارض مع مصدر قديم، سجله في `conflicts.md`
7. أضف سطر في `log.md`

### مثال prompt

```text
Read this new raw source.
Create a source-note.
Update index.md.
Update only the most relevant concept or entity page if necessary.
If the source conflicts with older material, record it in conflicts.md.
Append a short ingest entry to log.md.
Do not perform broad multi-page rewrites without asking first.
```

## ثالثًا: شلون تسأل النظام

مو كل سؤال له نفس workflow.

### 1. أسئلة التصفح

مثل:

- شنو المصادر المتعلقة بهالموضوع؟
- شنو أهم المفاهيم؟
- منو الكيانات المرتبطة؟

هني عادي يبدأ من الويكي.

### 2. أسئلة التحليل الاستكشافي

مثل:

- شنو الأنماط المتكررة؟
- وين نقاط الخلاف؟
- شنو الفرضيات الممكنة؟

هني الويكي يفيد، لكن إذا الاستنتاج له وزن، خله يفتح raw sources بعد.

### 3. الأسئلة الحساسة

مثل:

- claim دقيق
- timeline
- رقم
- مقارنة نهائية
- recommendation
- decision
- ترجيح بين مصدرين

هني لا تقبل answer من الويكي وحده.

### مثال prompt

```text
Use the wiki only as a routing layer.
Before giving the final answer, inspect the linked raw sources directly.
Quote or cite the relevant evidence.
If the evidence is incomplete or conflicting, say that clearly.
```

## رابعًا: شلون تعرف شنو ينحفظ وشنو لا

القاعدة الأساسية:

**مو كل answer ينحفظ**

اللي غالبًا يستحق الحفظ:

- comparison راح ترجع له
- concept page نضجت
- synthesis ناضج ومربوط بمصادر
- contradiction record
- framework فعلاً نافع

اللي غالبًا ما يستحق:

- جواب محادثة عادي
- رأي مؤقت
- تحليل أولي
- answer مبني على سياق جلسة فقط

### مثال prompt

```text
Do not save this answer automatically.
Only suggest promotion if the output is durable, source-linked, and likely to be reused.
If promoted, label it with the correct page type.
```

## خامسًا: شلون تستخدم conflicts.md بشكل صح

هذا الملف من أعلى قيمة في النظام إذا انضبط.

كل تعارض لازم يوثق:

- موضوع الخلاف
- المصدر الأول
- المصدر الثاني
- شنو التعارض بالضبط
- هل هو unresolved أو partially resolved أو resolved

### مثال prompt

```text
Do not resolve the contradiction too early.
Record the disagreement first.
Only mark it resolved if the raw evidence clearly settles it.
```

## سادسًا: شلون تسوي lint بدون ما يصير تجميل

الغلط الشائع:
النموذج يشوف اختلافات ويحاول "ينعمها" ويطلع شكل نظيف.

هذا غلط.

الصح:

- يدور broken links
- يدور orphan pages
- يدور pages ما فيها source links
- يدور syntheses قديمة
- يدور contradictions مفتوحة
- ويرجع للـ raw sources إذا احتاج يتحقق

### مثال prompt

```text
Run a grounded lint pass.
Do not smooth over contradictions.
Check for broken links, orphan pages, missing source links, outdated syntheses, and unresolved conflicts.
Validate important claims against raw sources.
```

## سابعًا: شلون تكبر النظام بدون ما ينهار

لا تفتح كل شي دفعة وحدة.

كبر بهالترتيب:

1. `source-note`
2. `index.md`
3. `conflicts.md`
4. `entity` pages المهمة
5. `concept` pages المهمة
6. `stable-synthesis` فقط لما يثبت إنه يستاهل

إذا كل مصدر جديد بدأ يلمس عشر صفحات، وقف.

هذا معناته إن الـ ingest قاعد يكبر أكثر من اللازم.

## الزبدة التشغيلية

إذا تبي الفكرة تنجح:

- خلك محافظ في ingest
- خل الويكي routing layer
- رجع للـ raw في الأسئلة المهمة
- لا تحفظ كل answer
- سجل التناقضات بدل ما تخفيها

هذا هو الفرق بين نظام معرفة ناضج
وبين ويكي مرتب شكليًا لكنه قاعد ينخدع بكلامه.
