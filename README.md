# JTV — سيرفر قنوات JassimTV

هذا المستودع هو **سيرفر البيانات** الذي يقرأ منه تطبيق JassimTV القنوات ودليل البث.
عدّل أي ملف هنا، وسيلتقط التطبيق التغيير تلقائيًا خلال أقل من دقيقة **دون إعادة تشغيل**.
أثناء التشغيل يفحص المشغّل القناة الحالية كل ~25 ثانية، ويعيد ضبط التوقيت بهدوء.

## هيكل المستودع

```
JTV/
├── القنوات/
│   ├── index.json          الملف العام لربط القنوات
│   ├── one.json            قاعدة بيانات + دليل بث قناة جاسم ون
│   ├── kids.json
│   ├── cinema.json
│   ├── docs.json
│   ├── music.json
│   └── sports.json
├── logos/                  شعارات القنوات
├── VOD/
│   ├── movies.m3u          قائمة أفلام
│   └── series.m3u          قائمة مسلسلات
└── README.md
```

## ربط قناة جديدة

1. أنشئ ملفًا داخل `القنوات/` مثل `news.json` وانسخ بنية أي قناة موجودة.
2. أضف سطرًا في `القنوات/index.json`:

```json
{ "id": "news", "file": "news.json" }
```

3. ضع شعار القناة في `logos/news.png` وأشِر إليه في `channel.logo`.
4. التطبيق يضيف القناة تلقائيًا في أول مزامنة.

## ملف القناة

```json
{
  "schema": 1,
  "revision": 2,
  "updated": "2026-11-23T05:13:33+03:00",
  "channel": {
    "id": "one",
    "name": "جاسم ون",
    "tagline": "منوعات على مدار الساعة",
    "logo": "https://raw.githubusercontent.com/qmsweb/JTV/main/logos/one.png",
    "accent": "#216AF1"
  },
  "broadcast": {
    "anchor": "2026-11-23T00:00:00+03:00",
    "loop": true
  },
  "items": [
    {
      "id": "bbb",
      "title": "مغامرة الأرنب",
      "description": "فيلم عائلي...",
      "url": "https://archive.org/...mp4",
      "duration": 596,
      "category": "عائلي"
    }
  ]
}
```

### كيف يُحسب البث؟

- `anchor`: لحظة بداية الدورة (ISO 8601 مع فرق التوقيت، أو `anchorMs`).
- `items`: تُشغَّل بالترتيب، و`duration` بالثواني.
- عند نهاية آخر عنصر تعود القناة للعنصر الأول تلقائيًا.
- غيّر `anchor` أو ترتيب `items` أو المدد، وسيُعيد التطبيق حساب «الآن» و«التالي» فورًا
  ثم يضبط موضع المشغّل على البث الحالي دون أن يرى المشاهد انقطاعًا.
- `revision` و`updated` للتوثيق فقط؛ التطبيق يكشف التغيير من محتوى الملف نفسه.

## VOD

مجلدا الأفلام والمسلسلات جاهزان للمرحلة القادمة من التطبيق، بصيغة M3U قياسية
(`#EXTM3U` + `#EXTINF`). أضف روابطك بنفس النمط.

## الروابط المباشرة

- الفهرس: `https://raw.githubusercontent.com/qmsweb/JTV/main/القنوات/index.json`
- قناة: `https://raw.githubusercontent.com/qmsweb/JTV/main/القنوات/one.json`
- شعار: `https://raw.githubusercontent.com/qmsweb/JTV/main/logos/one.png`

> المستودع عام (Public) حتى يستطيع التطبيق القراءة دون مفاتيح.
