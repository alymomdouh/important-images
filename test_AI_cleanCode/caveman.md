فكرت قبل كده إن الـ coding agent بتاعك بيضيع نص الـ tokens بتاعته على كلام زي “Sure! I’d be happy to help you with that”؟ فيه أداة حولت الموضوع ده لميم وحلته في نفس الوقت.
‎
الأداة اسمها Caveman وهي skill بتتسطب على الـ coding agent بتاعك، سواء Claude Code أو Cursor أو Copilot أو غيرهم. تخيل إنها بتقول للـ agent: متقولش كلام كتير، قول اللي محتاج تقوله وبس. النتيجة إن الـ output tokens بتقل بنسبة 65-75% من غير ما تخسر أي معلومة تقنية.
‎
الفكرة بسيطة. الأداة بتحط prompt مخصص في ملف SKILL.md بيقول للـ model: شيل الـ articles زي a وan وthe، شيل الـ filler words، شيل المجاملات، واستخدم أقصر كلمة توصل المعنى. نفترض إنك سألت عن bug في React component، بدل ما يرد عليك في فقرة كاملة عن الـ object references والـ shallow comparison، يقولك في سطر واحد: إيه المشكلة، إيه الحل. والكود نفسه مبيتغيرش، الـ caveman بيتكلم حوالين الكود بس.
‎
اللي شدني فيها إن فيه تلات مستويات ضغط: الـ Lite بيشيل الحشو بس ويسيب الجمل مترتبة، والـ Full ده الافتراضي وبيبدأ يكسر الجمل لـ fragments، والـ Ultra بيضغط لأقصى درجة زي التلغراف. وفيه كمان mode اسمه Wenyan بيحول الكلام للصينية الكلاسيكية عشان أقصى compression ممكنة. والنقطة المهمة إن الأداة بتفصل الـ compression تلقائي لما يكون فيه security warnings أو أوامر destructive.
‎
بصراحة، الميزة الأكبر مش بس التوفير في الفلوس. الردود بتبقى أسرع وأسهل في القراءة. وورقة بحثية من مارس 2026 لقت إن إجبار الـ models على الإيجاز حسّن الدقة بـ 26 نقطة مئوية في بعض الـ benchmarks. بس لازم تعرف إن الـ 75% دي على الـ output tokens بس، مش الـ session كلها. الـ input context اللي فيه تاريخ المحادثة والملفات مبيتأثرش، فالتوفير الفعلي في session كاملة أقرب لـ 25%.
‎
لو قارنتها بإنك تكتب system prompt بنفسك يقول للـ agent كن مختصراً، الفرق إن Caveman عندها قواعد دقيقة بتفرق بين الكلام العادي اللي بيتضغط والكود والـ error messages اللي بتفضل زي ما هي. ومعاها eval harness تقدر تقيس الفرق بنفسك.
‎
عشان تبدأ، أمر واحد: npx skills add JuliusBrussee/caveman وبعدها اكتب /caveman في الـ agent. الأداة مجانية ومفتوحة المصدر وعدت 20 ألف star على GitHub.
‎
بتدفعوا قد إيه على الـ output tokens في الـ agentic workflows بتاعتكم؟ وجربتوا تقللوا الحشو ده قبل كده؟
بحاول كل يوم أجيبلك أداة جديدة تستاهل تجربها


###    https://github.com/JuliusBrussee/caveman

<img width="800" height="1356" alt="image" src="https://github.com/user-attachments/assets/5b4a9199-b19a-47f0-bf65-1715029a16b2" />
