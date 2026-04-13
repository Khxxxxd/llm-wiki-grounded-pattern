# LLM Wiki: The Grounded Pattern

مو كل `LLM Wiki` فكرة صح.

المشكلة مو في بناء ويكي فوق ملفاتك.

المشكلة تبدأ لما الويكي يتحول من "خريطة" إلى "مصدر الحقيقة" العملي.

هالrepo معمول عشان يعطيك نسخة أنضج:

- `raw sources` تظل الحقيقة
- `wiki` تصير طبقة تنظيم وربط
- الإجابة المهمة ترجع للمصدر الخام قبل ما تعتمد عليها

## شنو تلقى هني؟

- [Grounded-Pattern.md](./Grounded-Pattern.md)
  هذا ملف الـ pattern نفسه. يشرح الفكرة بشكل high-level ونظيف.
- [AGENTS.md](./AGENTS.md)
  هذا ملف تشغيل عام. تقدر تستخدمه كما هو أو تعيد تسميته إلى `CODEX.md` أو `CLAUDE.md` أو `GEMINI.md` حسب أداتك.
- [docs/quick-start.md](./docs/quick-start.md)
  هذا شرح عملي سريع: شلون تبدأ، وشنو تسوي من أول يوم.
- [docs/implementation-playbook.md](./docs/implementation-playbook.md)
  هذا الجزء العملي الحقيقي. فيه workflow واضح: شلون تدخل مصدر، شلون تسأل، شلون تحفظ، ومتى توقف النموذج.
- [templates](./templates)
  قوالب جاهزة لصفحات الويكي.
- [examples](./examples)
  أمثلة لملفات مثل `index.md` و`log.md` و`conflicts.md`.

## شنو السالفه أصلًا؟

كثير ناس يستخدمون `RAG` الخام:

- يرفعون ملفات
- يسألون سؤال
- النموذج يبحث وقت السؤال
- ويرجع يجاوب

هذا ممتاز في أشياء كثيرة.

لكن فيه مشكلة واضحة:
إذا سؤالك يحتاج تركيب فهم عبر أكثر من مصدر، النموذج يعيد الشغل من الصفر كل مرة.

الرد الطبيعي على هالمشكلة كان:
"خل النموذج يبني ويكي متراكم فوق المصادر"

الفكرة نفسها مو غلط.

الغلط يصير إذا خليت الويكي:

- يختصر كل شي
- ويصير هو مدخل `query`
- وتخلي الإجابات الجديدة ترجع تنحفظ فيه
- وبعدها يعتمد عليها لاحقًا

هني تدخل في:

- تلخيص فوق تلخيص
- claims مختلطة مع summaries
- generated text يبني على generated text

وبعد فترة يصير النظام مرتب شكليًا، لكن معرفيًا قاعد يبتعد عن الواقع.

## الزبدة

إذا تبي هالفكرة تعيش معاك بشكل صح، لازم تفصل بين ثلاث طبقات:

### 1. الحقيقة

هذي في `raw sources`

- المقالات
- الـ PDFs
- الـ transcripts
- الملاحظات الأصلية
- أي evidence فعلي

### 2. التنظيم

هذي في `wiki`

- `source-note`
- `entity`
- `concept`
- `contradiction`
- `stable-synthesis`
- `provisional`

### 3. التشغيل

هذي في `schema`

يعني ملف مثل:

- `AGENTS.md`
- أو `CODEX.md`
- أو `CLAUDE.md`
- أو `GEMINI.md`

وهذا أهم ملف في المشروع كله، لأن هو اللي يقول للنموذج:

- متى يرجع للمصدر الخام
- متى يمنع الحفظ
- متى يستأذن
- شلون يكتب التناقضات
- وشنو يعتبر high-stakes

## ليش هالنسخة أفضل؟

لأنها ما تحاول تستبدل الحقيقة.

هي فقط ترتبها.

الويكي يفيدك في:

- التنقل
- الربط
- اكتشاف التناقضات
- تسريع الفهم
- بناء synthesis انتقائي

لكن إذا عندك:

- claim دقيق
- رقم
- تاريخ
- مقارنة مهمة
- recommendation
- قرار

فهني النموذج لازم يرجع للمصدر الخام.

## شلون تستخدم هالrepo؟

### إذا تبي تفهم الفكرة أول

ابدأ من:

- [Grounded-Pattern.md](./Grounded-Pattern.md)

### إذا تبي تنسخ النظام إلى مشروعك

ابدأ من:

- [AGENTS.md](./AGENTS.md)

ثم سمّه حسب بيئتك:

- `AGENTS.md`
- `CODEX.md`
- `CLAUDE.md`
- `GEMINI.md`

ثم عدل المسارات والقواعد حسب شغلك.

### إذا تبي تبدأ بسرعة

امش على:

- [docs/quick-start.md](./docs/quick-start.md)
- [docs/implementation-playbook.md](./docs/implementation-playbook.md)

## أهم قاعدة

لا تحفظ كل answer.

هذي من أكثر الأشياء اللي تخرب النظام.

اللي ينحفظ فقط هو:

- synthesis ناضج
- comparison عالي القيمة
- concept page واضح
- contradiction record
- note راح ترجع لها مره ثانية

أما الكلام العابر، خله عابر.

## هيكل مقترح للمجلدات

```text
knowledge-base/
├── raw/
│   ├── articles/
│   ├── pdfs/
│   ├── transcripts/
│   └── notes/
├── wiki/
│   ├── index.md
│   ├── log.md
│   ├── conflicts.md
│   ├── entities/
│   ├── concepts/
│   ├── sources/
│   ├── syntheses/
│   └── provisional/
├── AGENTS.md
├── CODEX.md
├── CLAUDE.md
└── GEMINI.md
```

## شنو لازم تسويه الحين؟

1. انسخ `Grounded-Pattern.md` كمرجع للفكرة.
2. انسخ schema file مناسب لأداتك:
   `AGENTS.md` أو `CODEX.md` أو `CLAUDE.md` أو `GEMINI.md`
3. ابنِ مجلد `raw/` منفصل وواضح.
4. استخدم القوالب داخل `templates/`.
5. ابدأ بـ `index.md` و`log.md` و`conflicts.md`.
6. لا تسمح للويكي أن يصير بديل عن المصدر.

## آخر كلام

إذا بتسوي `LLM Wiki`، لا تبني "ويكي ذكي" فقط.

ابنِ نظام فيه:

- truth boundary
- routing layer
- operating rules

إذا ضبطت هالثلاثة، هني فعلاً تبدأ المعرفة تتراكم بشكل نافع بدل ما تتحول إلى لعبة هاتف أنيقة.
