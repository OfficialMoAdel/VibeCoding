# agents-md-setup

مستودع بسيط لإدارة ملفات القواعد الخاصة بالـ AI coding agents، بحيث يكون عندك مصدر واحد أساسي للتعليمات، ثم يتم نشره محليًا في المسارات التي تستخدمها الأدوات المختلفة.

الفكرة هنا أن الملف الأساسي في هذا الـ repo هو:

- `AGENTS.md`
- `generate-agents.md`

بحيث:
- `AGENTS.md` يكون المصدر الأساسي للقواعد والتعليمات.
- `generate-agents.md` يتولى تجهيز أو إنشاء الملفات اللازمة للمشروع إذا كان المشروع قائمًا بالفعل.
- يتم نسخ `AGENTS.md` إلى مسار OpenCode كما هو.
- ويتم نسخ نفس المحتوى إلى Claude Code لكن باسم `CLAUDE.md`.

## لماذا هذا المستودع؟

الهدف هو تجنب تكرار نفس التعليمات في أكثر من مكان أو أكثر من أداة.

بدل ما تكتب نفس القواعد في:
- OpenCode
- Claude Code
- كل مشروع جديد

يكون عندك مصدر واحد واضح داخل repo، ثم تنشره محليًا في المسارات المناسبة.

هذا مفيد خصوصًا إذا كنت:
- تستخدم أكثر من agent
- تريد الحفاظ على نفس أسلوب العمل بين الأدوات
- تريد تحديث ملف واحد فقط بدل تعديل أكثر من نسخة

## الملفات داخل المستودع

- `AGENTS.md`  
  الملف الأساسي الذي يحتوي على القواعد والتعليمات العامة.

- `generate-agents.md`  
  ملف مساعد يمكن استدعاؤه لتوليد أو تجهيز ملفات التعليمات المطلوبة للمشروع القائم.

## المسارات المستهدفة

### OpenCode

#### Windows
```powershell
C:\Users\<username>\.config\opencode\AGENTS.md
```

#### Linux
```bash
~/.config/opencode/AGENTS.md
```

OpenCode يدعم `AGENTS.md` كملف global rules، ويستخدمه عبر الجلسات كلها  .

### Claude Code

#### Windows
```powershell
C:\Users\<username>\.claude\CLAUDE.md
```

#### Linux
```bash
~/.claude/CLAUDE.md
```

Claude Code يستخدم `CLAUDE.md` كملف user instructions على مستوى المستخدم، وClaude يحمّل هذا الملف في كل Session  .

## آلية العمل

الفكرة أن هذا المستودع يحتفظ بالنسخة الأساسية في:

```text
AGENTS.md
```

ثم يتم:

1. نسخ الملف إلى مسار OpenCode بنفس الاسم:
   - `AGENTS.md`

2. نسخ نفس الملف إلى مسار Claude Code مع تغيير الاسم إلى:
   - `CLAUDE.md`

وبالتالي يكون عندك مصدر واحد للحقيقة، لكن متوافق مع الأداتين.

## لماذا تغيير الاسم في Claude؟

لأن Claude Code لا يقرأ `AGENTS.md` مباشرة كملف التعليمات الأساسي، وإنما يعتمد على `CLAUDE.md`  .  
وفي المقابل، OpenCode يعتمد على `AGENTS.md`، مع دعم `CLAUDE.md` كـ fallback فقط عند عدم وجود `AGENTS.md`  .

لذلك الأفضل هنا:
- الاحتفاظ بالملف الأساسي في repo باسم `AGENTS.md`
- ثم نسخه إلى Claude Code باسم `CLAUDE.md`

## generate-agents.md

الملف `generate-agents.md` مخصص ليتم استدعاؤه عندما تريد تجهيز مشروع قائم بالفعل.

الفكرة منه:
- فحص المشروع الحالي
- فهم هيكل المشروع أو احتياجاته
- ثم إنشاء الملفات اللازمة للتعليمات والقواعد
- مع الحفاظ على وجود `AGENTS.md` كمصدر أساسي داخل هذا repo

بمعنى آخر، بدل ما تكتب كل شيء يدويًا في كل مشروع، يتم استخدام `generate-agents.md` كمدخل أو template أو workflow يساعد على إنشاء ملفات المشروع المطلوبة بشكل أسرع.

## ملاحظات مهمة

- إذا كنت تستخدم OpenCode وClaude Code معًا، فهذه الطريقة تمنع تكرار نفس المحتوى في أكثر من ملف.
- OpenCode يعطي أولوية لـ `~/.config/opencode/AGENTS.md` على `~/.claude/CLAUDE.md` إذا وُجد الاثنان  .
- في Claude Code، الأفضل أن يكون الملف مختصرًا وواضحًا، لأن الملفات الطويلة تستهلك context أكثر وقد تقلل الالتزام بالتعليمات  .



## مثال سريع

### Linux
```bash
cp AGENTS.md ~/.config/opencode/AGENTS.md
cp AGENTS.md ~/.claude/CLAUDE.md
```

### Windows PowerShell
```powershell
Copy-Item ".\AGENTS.md" "$HOME\.config\opencode\AGENTS.md" -Force
Copy-Item ".\AGENTS.md" "$HOME\.claude\CLAUDE.md" -Force
```

## داخل المشروع: استخدام symbolic link

لو كنت تريد استخدام نفس الملف داخل المشروع مع أكثر من أداة بدون تكرار المحتوى، يمكنك جعل `CLAUDE.md` مجرد symbolic link يشير إلى `AGENTS.md`.

بهذه الطريقة:
- `AGENTS.md` يظل هو الملف الأساسي
- `CLAUDE.md` لا يحتوي على نسخة ثانية من نفس المحتوى
- أي تعديل على `AGENTS.md` ينعكس مباشرة على `CLAUDE.md`

### Linux
```bash
ln -s AGENTS.md CLAUDE.md
```

### Windows PowerShell
```powershell
New-Item -ItemType SymbolicLink -Path .\CLAUDE.md -Target .\AGENTS.md
```

### Windows CMD
```cmd
mklink CLAUDE.md AGENTS.md
```

> ملاحظة: إذا كان `CLAUDE.md` موجودًا بالفعل، احذفه أولًا قبل إنشاء الـ symbolic link.