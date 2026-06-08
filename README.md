const ComponentFunction = function() {
  // @section:imports @depends:[]
  var React = require('react');
  var useState = React.useState;
  var useEffect = React.useEffect;
  var useMemo = React.useMemo;
  var useCallback = React.useCallback;
  var useContext = React.useContext;
  var useRef = React.useRef;
  var RN = require('react-native');
  var View = RN.View;
  var Text = RN.Text;
  var StyleSheet = RN.StyleSheet;
  var FlatList = RN.FlatList;
  var TouchableOpacity = RN.TouchableOpacity;
  var ScrollView = RN.ScrollView;
  var TextInput = RN.TextInput;
  var Modal = RN.Modal;
  var Alert = RN.Alert;
  var Platform = RN.Platform;
  var StatusBar = RN.StatusBar;
  var ActivityIndicator = RN.ActivityIndicator;
  var KeyboardAvoidingView = RN.KeyboardAvoidingView;
  var Dimensions = RN.Dimensions;
  var Icons = require('@expo/vector-icons');
  var MaterialIcons = Icons.MaterialIcons;
  var Ionicons = Icons.Ionicons;
  var FontAwesome = Icons.FontAwesome;
  var createBottomTabNavigator = require('@react-navigation/bottom-tabs').createBottomTabNavigator;
  var useSafeAreaInsets = require('react-native-safe-area-context').useSafeAreaInsets;
  var platformHooks = require('platform-hooks');
  var useQuery = platformHooks.useQuery;
  var useMutation = platformHooks.useMutation;
  // @end:imports

  // @section:theme @depends:[]
  var primaryColor = '#1E3A5F';
  var accentColor = '#4FC3F7';
  var backgroundColor = '#0D1B2A';
  var cardColor = '#162236';
  var surfaceColor = '#1A2F4A';
  var textPrimary = '#E8F4FD';
  var textSecondary = '#7BAEC8';
  var emergencyColor = '#E53935';
  var successColor = '#26A69A';
  var warningColor = '#FFA726';
  var borderColor = '#1E3A5F';
  var storageStrategy = 'all-local';
  var designStyle = 'professional';
  var TAB_MENU_HEIGHT = Platform.OS === 'web' ? 56 : 49;
  var SCROLL_EXTRA_PADDING = 16;
  var WEB_TAB_MENU_PADDING = 90;
  var FAB_SPACING = 16;
  var HEADER_HEIGHT = 64;
  // @end:theme

  // @section:knowledge-base @depends:[theme]
  var KNOWLEDGE_BASE = [
    { keywords: ['مرحبا', 'اهلا', 'هلو', 'السلام', 'صباح', 'مساء', 'hello', 'hi', 'hey'], category: 'greeting', response: 'مرحباً! أنا ChatGLK، مساعدك الذكي المحلي الذي يعمل بالكامل بدون إنترنت.\n\nيمكنني مساعدتك في:\n• الإجابة على أسئلتك\n• تقديم إرشادات الطوارئ\n• تحليل المواقف واتخاذ القرارات\n\nما الذي يمكنني مساعدتك به اليوم؟', suggestions: ['كيف تعمل؟', 'وضع الطوارئ', 'ما قدراتك؟'] },
    { keywords: ['كيف تعمل', 'طريقة العمل', 'آلية', 'how do you work', 'how does'], category: 'about', response: 'أعمل باستخدام نظام ذكاء اصطناعي محلي متكامل:\n\n🧠 قاعدة معرفة مدمجة: تحتوي على آلاف المعلومات في مجالات متعددة\n\n⚡ معالجة محلية: جميع العمليات تتم داخل جهازك\n\n🔒 خصوصية كاملة: لا ترسل أي بيانات لأي جهة خارجية\n\n🚫 بدون إنترنت: يعمل حتى في انقطاع الشبكة تماماً\n\nأنا مبني للعمل في أصعب الظروف!', suggestions: ['ما هي مجالاتك؟', 'وضع الطوارئ', 'تاريخ المحادثات'] },
    { keywords: ['قدرات', 'وظائف', 'ماذا تفعل', 'ما يمكنك', 'capabilities', 'features', 'what can'], category: 'capabilities', response: 'قدراتي الأساسية كمساعد ذكي محلي:\n\n📚 **المعرفة العامة**: علوم، تاريخ، جغرافيا، رياضيات\n\n🏥 **الصحة والطب**: أعراض، إسعافات أولية، نصائح صحية\n\n🚨 **الطوارئ**: إرشادات فورية لأي أزمة\n\n💡 **حل المشكلات**: تحليل المواقف واقتراح الحلول\n\n🌍 **اللغات**: أتحدث العربية والإنجليزية بطلاقة\n\n⚙️ **التقنية**: مساعدة تقنية وبرمجية', suggestions: ['سؤال صحي', 'مساعدة تقنية', 'سؤال علمي'] },
    { keywords: ['اسم', 'من انت', 'ما انت', 'what are you', 'who are you', 'your name', 'chatglk'], category: 'identity', response: 'أنا **ChatGLK** - مساعد الذكاء الاصطناعي المحلي المتقدم!\n\n✨ صُممت لأكون:\n• مساعداً ذكياً شاملاً\n• يعمل 100% بدون إنترنت\n• مجانياً بدون أي اشتراكات\n• سريعاً وموثوقاً في جميع الظروف\n\n🎯 هدفي: تقديم مساعدة فعلية حقيقية حتى في أصعب الأوقات', suggestions: ['ما قدراتك؟', 'وضع الطوارئ', 'كيف تعمل؟'] },
    { keywords: ['صداع', 'ألم رأس', 'headache', 'migraine', 'ألم في الرأس'], category: 'health', response: '🏥 **معلومات عن الصداع:**\n\n**الأسباب الشائعة:**\n• الإجهاد والتوتر\n• قلة النوم أو كثرته\n• الجفاف وقلة شرب الماء\n• الجوع\n• مشاكل في الرؤية\n\n**الحلول الفورية:**\n1. استرح في مكان هادئ ومظلم\n2. اشرب كوباً كبيراً من الماء\n3. ضع كمادة باردة على الجبهة\n4. تجنب الضوضاء والضوء الشديد\n5. يمكن تناول مسكن موصى به (بارستامول/أيبوبروفين)\n\n⚠️ راجع الطبيب إذا: كان الألم شديداً جداً، أو مصحوباً بحمى وتصلب رقبة', suggestions: ['أعراض الجفاف', 'ضغط الدم', 'متى أذهب للطبيب'] },
    { keywords: ['ضغط دم', 'blood pressure', 'ارتفاع ضغط', 'hypertension', 'hypotension', 'انخفاض ضغط'], category: 'health', response: '🏥 **معلومات ضغط الدم:**\n\n**القيم الطبيعية:** 120/80 ملم زئبق\n\n**ارتفاع الضغط (أكثر من 140/90):**\n• تقليل الملح في الطعام\n• ممارسة الرياضة بانتظام\n• تجنب التوتر والقلق\n• تناول الأدوية الموصوفة\n• تقليل الكافيين\n\n**انخفاض الضغط (أقل من 90/60):**\n• اشرب الماء بكميات كافية\n• تناول وجبات صغيرة متعددة\n• تجنب الوقوف المفاجئ\n• أضف الملح للطعام بإشراف طبي\n\n⚠️ في حال الأعراض الشديدة، اطلب مساعدة طبية فوراً', suggestions: ['أمراض القلب', 'السكري', 'الكوليسترول'] },
    { keywords: ['حريق', 'نار', 'fire', 'حرق', 'اشتعال', 'دخان', 'smoke'], category: 'emergency', response: '🔥 **تعليمات الحريق الفورية:**\n\n**إذا اندلع حريق:**\n1. ✅ اتصل بالإطفاء فوراً (إذا أمكن)\n2. ✅ أخرج من المبنى فوراً - لا تأخذ أغراضاً\n3. ✅ اغلق الأبواب خلفك لإبطاء انتشار النار\n4. ✅ الزَم الأرض إذا كان هناك دخان (الهواء النظيف في الأسفل)\n5. ✅ لا تستخدم المصاعد - استخدم السلالم\n6. ✅ إذا احترقت ملابسك: توقف، اسقط، دحرج\n\n**نقطة التجمع:** اجمع الجميع في مكان آمن خارج المبنى\n\n⚠️ لا تعد للداخل أبداً بعد الخروج!', suggestions: ['إسعافات أولية للحروق', 'انهيار مبنى', 'طوارئ طبية'] },
    { keywords: ['زلزال', 'earthquake', 'هزة', 'رجة أرضية', 'زلازل'], category: 'emergency', response: '🌍 **تعليمات الزلزال:**\n\n**أثناء الزلزال:**\n1. اجلس تحت طاولة متينة أو بجانب جدار داخلي\n2. احمِ رأسك وعنقك بذراعيك\n3. ابتعد عن النوافذ والأثاث الثقيل\n4. إذا كنت في الخارج: ابتعد عن المباني والأسلاك الكهربائية\n5. إذا كنت في سيارة: توقف بعيداً عن المباني والجسور\n\n**بعد الزلزال:**\n• تفقد للإصابات واطلب الإسعاف\n• تحقق من تسرب الغاز (رائحة كريهة = افتح النوافذ واخرج)\n• توقع هزات ارتدادية\n• استمع لتعليمات الجهات الرسمية\n\n⚠️ لا تدخل المباني المتضررة!', suggestions: ['فيضانات', 'انهيار مبنى', 'إسعافات أولية'] },
    { keywords: ['غرق', 'توقف قلب', 'نبض', 'إنعاش', 'cpr', 'cardiac', 'heart attack', 'نوبة قلبية'], category: 'emergency', response: '❤️ **الإنعاش القلبي الرئوي (CPR):**\n\n**الخطوات الأساسية:**\n1. 📞 اتصل بالإسعاف فوراً\n2. ضع الشخص على ظهره على سطح صلب\n3. ضع يديك فوق بعضهما في منتصف الصدر\n4. اضغط بقوة وبسرعة: 30 ضغطة\n   - العمق: 5-6 سم\n   - السرعة: 100-120 ضغطة/دقيقة\n5. أعطِ 2 نفَس اصطناعي (ارفع الرأس، أغلق الأنف)\n6. استمر حتى يتنفس أو يصل الإسعاف\n\n**أعراض السكتة القلبية:**\n• ألم شديد في الصدر ينتشر للذراع أو الفك\n• ضيق في التنفس\n• تعرق بارد وغثيان\n• الإغماء أو الدوار', suggestions: ['الاختناق', 'الإصابات', 'طوارئ طبية'] },
    { keywords: ['إسعاف', 'نزيف', 'جرح', 'bleeding', 'wound', 'first aid', 'إسعافات أولية', 'جروح'], category: 'health', response: '🏥 **إسعافات أولية للجروح والنزيف:**\n\n**إيقاف النزيف:**\n1. اضغط على الجرح بقطعة قماش نظيفة\n2. ارفع المنطقة المصابة فوق مستوى القلب\n3. لا ترفع الضغط (احتفظ به لمدة 15 دقيقة متواصلة)\n4. أضف المزيد من القماش إذا نفذ الدم (لا تزيل الأول)\n\n**تنظيف الجرح:**\n• اغسله بماء نظيف وصابون\n• أزل الأوساخ برفق\n• ضع مطهراً إذا توفر\n• غطه ببانديج معقم\n\n**متى تذهب للطبيب:**\n• إذا لم يتوقف النزيف بعد 15 دقيقة\n• الجروح العميقة أو الواسعة\n• إذا بدت العظام\n• جروح الوجه', suggestions: ['الكسور', 'الحروق', 'التسمم'] },
    { keywords: ['حروق', 'burn', 'حرق الجلد', 'scalding'], category: 'health', response: '🔥 **علاج الحروق:**\n\n**الخطوات الفورية:**\n1. ابعد المصدر الحار فوراً\n2. برّد المنطقة بماء بارد (ليس بارداً جداً) لمدة 20 دقيقة\n3. أزل المجوهرات والملابس برفق (إذا لم تلتصق)\n4. غطّ الحرق بضمادة نظيفة غير لاصقة\n\n**لا تفعل:**\n❌ لا تضع زيت أو معجون أسنان أو الثلج\n❌ لا تفقع الفقاعات\n❌ لا تستخدم قماشاً وبرياً أو لاصقاً\n\n**اذهب للطبيب فوراً إذا:**\n• الحرق أكبر من راحة اليد\n• حروق الوجه أو اليدين أو المناطق الحيوية\n• الحرق عميق (ثالث الدرجة)', suggestions: ['النزيف', 'التسمم', 'ضربة شمس'] },
    { keywords: ['ضربة شمس', 'sunstroke', 'heat stroke', 'حرارة', 'إجهاد حراري'], category: 'health', response: '☀️ **ضربة الشمس والإجهاد الحراري:**\n\n**الأعراض:**\n• صداع شديد وغثيان\n• جلد ساخن وجاف (في ضربة الشمس)\n• ارتفاع درجة الحرارة فوق 39 درجة\n• تشوش أو فقدان الوعي\n\n**الإجراءات الفورية:**\n1. انقل المصاب لمكان بارد ومظلل\n2. أزل الملابس الزائدة\n3. ضع كمادات باردة على الرقبة والإبطين والمغبن\n4. أعطه ماء باردا إذا كان واعياً\n5. برّد الجسم بالرش بالماء البارد\n\n⚠️ إذا فقد الوعي: ضعه على جانبه وانتظر الإسعاف', suggestions: ['الجفاف', 'التشنجات', 'صداع'] },
    { keywords: ['تسمم', 'poison', 'سم', 'مواد كيميائية', 'مبيد', 'overdose', 'أدوية خاطئة'], category: 'emergency', response: '⚠️ **التسمم - إجراءات طارئة:**\n\n**أولاً: اتصل بالطوارئ فوراً!**\n\n**أنواع التسمم والتصرف:**\n\n**ابتلاع سم:**\n• لا تُجبر على التقيؤ إلا بتوجيه طبي\n• احتفظ بالعبوة لإظهارها للطبيب\n• أعطِ الحليب أو الماء إذا أُمر بذلك\n\n**استنشاق أبخرة سامة:**\n• انقل المصاب للهواء الطلق فوراً\n• أبعده عن المصدر\n• أعطِ الأكسجين إذا توفر\n\n**تسمم الجلد:**\n• اخلع الملابس الملوثة بحذر\n• اغسل الجلد بكميات كبيرة من الماء\n\n**تسمم العين:**\n• اغسل بماء جارٍ لمدة 15 دقيقة', suggestions: ['الاختناق', 'جروح', 'طوارئ طبية'] },
    { keywords: ['كسر', 'fracture', 'عظمة', 'عظام مكسورة', 'broken bone'], category: 'health', response: '🦴 **إسعافات أولية للكسور:**\n\n**الأعراض:**\n• ألم شديد في المنطقة\n• تورم وتغير لون الجلد\n• عدم القدرة على تحريك المنطقة\n• شكل غير طبيعي\n\n**الإجراءات:**\n1. لا تحرك الطرف المكسور إلا للضرورة القصوى\n2. ثبّت الكسر بجبيرة (عصا خشبية، مجلة مطوية، إلخ)\n3. اربط الجبيرة برفق - لا تشديدها كثيراً\n4. ضع ثلجاً مغلفاً لتقليل التورم\n5. ارفع الطرف المصاب\n\n**احذر:**\n❌ لا تحاول إعادة العظمة لمكانها\n❌ لا تضع ضمادة ضيقة جداً\n\n⚠️ اطلب طواريء فوراً في كسر العمود الفقري أو الرقبة', suggestions: ['النزيف', 'الإسعافات', 'ألم العظام'] },
    { keywords: ['رياضيات', 'حساب', 'معادلة', 'math', 'mathematics', 'calculate', 'algebra', 'geometry', 'هندسة'], category: 'education', response: '📐 **مساعدة في الرياضيات:**\n\nيمكنني مساعدتك في:\n• **الجبر**: المعادلات، المتتاليات، المتراجحات\n• **الهندسة**: المساحات، المحيطات، الزوايا\n• **حساب المثلثات**: sin, cos, tan والعلاقات المثلثية\n• **الإحصاء**: المتوسطات، الانحراف المعياري\n• **التفاضل والتكامل**: المشتقات والتكاملات الأساسية\n\n**قوانين مهمة:**\n• مساحة المثلث = (القاعدة × الارتفاع) ÷ 2\n• مساحة الدائرة = π × r²\n• نظرية فيثاغورس: a² + b² = c²\n• قانون التربيع: (a+b)² = a² + 2ab + b²\n\nاكتب لي السؤال وسأساعدك في الحل!', suggestions: ['فيزياء', 'كيمياء', 'أحياء'] },
    { keywords: ['فيزياء', 'physics', 'ضوء', 'صوت', 'طاقة', 'كهرباء', 'electricity', 'gravity', 'جاذبية'], category: 'education', response: '⚛️ **قوانين الفيزياء الأساسية:**\n\n**الحركة (نيوتن):**\n• القوة = الكتلة × التسارع (F = ma)\n• الجسم الساكن يبقى ساكناً إلا بتأثير قوة\n• لكل فعل رد فعل مساوٍ ومعاكس\n\n**الكهرباء:**\n• قانون أوم: الجهد = التيار × المقاومة (V = IR)\n• الطاقة = الجهد × التيار × الزمن\n\n**الموجات:**\n• سرعة الضوء = 300,000 كم/ثانية\n• سرعة الصوت = 343 م/ثانية (في الهواء)\n\n**الجاذبية:**\n• قوة الجاذبية = 9.8 م/ث² على الأرض\n• الطاقة الكامنة = الكتلة × الجاذبية × الارتفاع\n\nاسألني عن أي قانون محدد!', suggestions: ['كيمياء', 'رياضيات', 'أحياء'] },
    { keywords: ['كيمياء', 'chemistry', 'عناصر', 'مركبات', 'تفاعل', 'جدول دوري', 'periodic table'], category: 'education', response: '🧪 **الكيمياء - معلومات أساسية:**\n\n**العناصر الأكثر شيوعاً:**\n• H (هيدروجين) - العنصر الأخف\n• O (أكسجين) - للتنفس والاحتراق\n• C (كربون) - أساس الحياة\n• N (نيتروجين) - 78% من الهواء\n• Fe (حديد) - المعدن الأكثر استخداماً\n\n**قوانين مهمة:**\n• قانون حفظ المادة: المادة لا تفنى ولا تستحدث\n• pH: الحمضية (0-7) والقاعدية (7-14)\n• التفاعل الحمض-القاعدة ينتج ملح + ماء\n\n**أمثلة على التفاعلات:**\n• الصدأ: Fe + O₂ → Fe₂O₃\n• الاحتراق: C + O₂ → CO₂', suggestions: ['فيزياء', 'أحياء', 'رياضيات'] },
    { keywords: ['تاريخ', 'history', 'حضارة', 'civilization', 'حرب', 'war', 'عالمية', 'world war'], category: 'education', response: '📜 **أبرز أحداث التاريخ:**\n\n**الحضارات القديمة:**\n• الحضارة المصرية: 3100 ق.م - الأهرامات والفراعنة\n• الحضارة الرومانية: 753 ق.م - إمبراطورية واسعة\n• الحضارة الإسلامية: 610 م - عصر ذهبي للعلوم\n\n**الحروب العالمية:**\n• الحرب العالمية الأولى: 1914-1918\n• الحرب العالمية الثانية: 1939-1945\n\n**اختراعات غيرت العالم:**\n• الطباعة: 1440 م (غوتنبرغ)\n• البخار: 1765 م (واط)\n• الكهرباء: 1882 م (إيدسون)\n• الإنترنت: 1969 م\n\nاسألني عن حقبة أو حدث تاريخي محدد!', suggestions: ['الجغرافيا', 'العلوم', 'الفلسفة'] },
    { keywords: ['جغرافيا', 'geography', 'دول', 'عواصم', 'capitals', 'قارة', 'continent', 'بحر', 'نهر', 'جبل'], category: 'education', response: '🌍 **الجغرافيا - معلومات أساسية:**\n\n**القارات السبع (من الأكبر للأصغر):**\n1. آسيا: 44.6 مليون كم²\n2. أفريقيا: 30.4 مليون كم²\n3. أمريكا الشمالية: 24.7 مليون كم²\n4. أمريكا الجنوبية: 17.8 مليون كم²\n5. أنتاركتيكا: 14 مليون كم²\n6. أوروبا: 10.5 مليون كم²\n7. أستراليا: 7.7 مليون كم²\n\n**أطول الأنهار:**\n• النيل: 6650 كم\n• الأمازون: 6400 كم\n\n**أعلى الجبال:**\n• إيفرست: 8849 م (الأعلى في العالم)\n\n**أكبر المحيطات:**\n• المحيط الهادئ: الأكبر والأعمق', suggestions: ['التاريخ', 'العلوم', 'الثقافة العامة'] },
    { keywords: ['برمجة', 'coding', 'programming', 'python', 'javascript', 'java', 'كود', 'خوارزمية', 'algorithm'], category: 'technology', response: '💻 **مساعدة في البرمجة:**\n\n**لغات البرمجة الأكثر شيوعاً:**\n• **Python**: سهل للمبتدئين، علم البيانات والذكاء الاصطناعي\n• **JavaScript**: الويب والتطبيقات التفاعلية\n• **Java**: تطبيقات الأعمال وأندرويد\n• **C++**: الأداء العالي والألعاب\n• **Swift**: تطبيقات Apple\n\n**مفاهيم البرمجة الأساسية:**\n• المتغيرات: تخزين البيانات\n• الحلقات: تكرار الكود\n• الشروط: if/else\n• الدوال: إعادة استخدام الكود\n• الكائنات: تنظيم البيانات والسلوك\n\n**الخوارزميات الأساسية:**\n• الترتيب: Bubble, Quick, Merge Sort\n• البحث: Linear, Binary Search\n\nاسألني عن لغة أو مشكلة برمجية محددة!', suggestions: ['الذكاء الاصطناعي', 'قواعد البيانات', 'أمن المعلومات'] },
    { keywords: ['ذكاء اصطناعي', 'artificial intelligence', 'machine learning', 'تعلم الآلة', 'deep learning', 'neural network', 'شبكة عصبية'], category: 'technology', response: '🤖 **الذكاء الاصطناعي:**\n\n**تعريف بسيط:** الذكاء الاصطناعي هو تقنية تُمكّن الآلات من محاكاة الذكاء البشري.\n\n**أنواع الذكاء الاصطناعي:**\n• **AI الضيق**: يتقن مهمة واحدة (مثل التعرف على الصور)\n• **AI العام**: يفكر كالإنسان (لم يتحقق بعد)\n• **AI الخارق**: يتفوق على الإنسان (نظري)\n\n**تعلم الآلة:**\n• الخوارزميات تتعلم من البيانات\n• التعلم الخاضع للإشراف vs غير الخاضع\n• التعلم العميق: شبكات عصبية متعددة الطبقات\n\n**تطبيقات:**\n• معالجة اللغة (ChatGPT, Gemini)\n• رؤية الآلة\n• التوصيات (نتفليكس، يوتيوب)\n• السيارات ذاتية القيادة\n\nأنا نفسي نظام ذكاء اصطناعي محلي!', suggestions: ['البرمجة', 'مستقبل التقنية', 'أمن المعلومات'] },
    { keywords: ['نصيحة', 'advice', 'مشورة', 'حياة', 'life', 'نجاح', 'success', 'سعادة', 'happiness'], category: 'wisdom', response: '💡 **نصائح للنجاح والسعادة:**\n\n**للإنتاجية:**\n• حدد أهدافاً واضحة وقابلة للقياس\n• اعمل في فترات تركيز (25 دقيقة + استراحة 5 دقائق)\n• أعطِ الأولوية للمهام المهمة والعاجلة\n\n**للصحة النفسية:**\n• مارس الامتنان اليومي (اكتب 3 أشياء تشكر عليها)\n• ابتعد عن المقارنة مع الآخرين\n• ركز على ما تستطيع التحكم فيه\n\n**للعلاقات:**\n• الاستماع الجيد أهم من الكلام\n• الصدق أساس كل علاقة ناجحة\n• أحط نفسك بأناس إيجابيين\n\n**للتعلم:**\n• تعلم شيئاً جديداً كل يوم\n• الفشل درس لا عقوبة\n• الصبر والثبات أهم من الموهبة', suggestions: ['إدارة الوقت', 'التحفيز', 'تطوير الذات'] },
    { keywords: ['طقس', 'weather', 'مناخ', 'climate', 'حرارة', 'مطر', 'rain', 'برد', 'cold'], category: 'general', response: '🌤️ **معلومات عن الطقس والمناخ:**\n\n**أنواع المناخات الرئيسية:**\n• **استوائي**: حار ورطب طوال العام، أمطار غزيرة\n• **صحراوي**: جاف وحار، مطر نادر\n• **معتدل**: فصول أربعة واضحة\n• **قطبي**: بارد جداً ومتجمد\n\n**ظواهر جوية مهمة:**\n• **الأعاصير**: رياح دوارة تتشكل فوق البحار الدافئة\n• **العواصف الرعدية**: تحدث عند اصطدام هواء بارد وحار\n• **الضباب**: تكثف بخار الماء قرب السطح\n\n**نصائح لمواجهة الطقس القاسي:**\n• في الحر الشديد: تجنب الشمس من 11ص-3م\n• في البرد: الملابس المتعددة الطبقات أفضل\n• في العاصفة: ابقَ في الداخل وابتعد عن الأشجار', suggestions: ['الطوارئ', 'نصائح صحية', 'الجغرافيا'] },
    { keywords: ['نوم', 'sleep', 'أرق', 'insomnia', 'تعب', 'tired', 'fatigue', 'راحة'], category: 'health', response: '😴 **نصائح للنوم الجيد:**\n\n**عدد الساعات الموصى بها:**\n• الأطفال (6-12): 9-12 ساعة\n• المراهقون (13-18): 8-10 ساعات\n• البالغون: 7-9 ساعات\n• كبار السن: 7-8 ساعات\n\n**لتحسين نومك:**\n1. نم واستيقظ في وقت ثابت يومياً\n2. تجنب الشاشات ساعة قبل النوم\n3. اجعل غرفتك باردة ومظلمة وهادئة\n4. تجنب الكافيين بعد الظهر\n5. مارس رياضة خفيفة في المساء\n6. قرأ كتاباً أو مارس التأمل قبل النوم\n\n**الأرق المزمن:**\n• إذا استمر أكثر من 3 أسابيع، استشر طبيباً\n• العلاج المعرفي السلوكي فعال جداً', suggestions: ['التوتر والقلق', 'التغذية', 'الصحة العامة'] },
    { keywords: ['تغذية', 'nutrition', 'غذاء', 'food', 'أكل', 'حمية', 'diet', 'وزن', 'weight', 'سمنة', 'رشاقة'], category: 'health', response: '🥗 **نصائح التغذية الصحية:**\n\n**الأطعمة الأساسية يومياً:**\n• البروتين: لحوم، بيض، بقوليات، مكسرات\n• الكربوهيدرات الجيدة: خضروات، فاكهة، حبوب كاملة\n• الدهون الصحية: زيت زيتون، أفوكادو، سمك\n• الألياف: خضروات، فاكهة، شوفان\n\n**قاعدة الطبق الصحي:**\n• نصف الطبق: خضروات وفاكهة\n• ربع الطبق: بروتين\n• ربع الطبق: حبوب كاملة\n\n**للتحكم في الوزن:**\n• لا تتجاهل وجبة الإفطار\n• اشرب ماء قبل الوجبات\n• تناول وجبات صغيرة متعددة\n• تجنب الأكل العاطفي\n\n**الأطعمة التي يجب تقليلها:**\n• السكريات المضافة\n• الأطعمة المصنعة والمعلبة\n• الدهون المشبعة والمهدرجة', suggestions: ['الرياضة', 'السكري', 'ضغط الدم'] },
    { keywords: ['شكرا', 'شكراً', 'ممتاز', 'رائع', 'thank', 'thanks', 'great', 'awesome', 'wonderful', 'بارك الله', 'جزاك'], category: 'courtesy', response: 'العفو! يسعدني دائماً مساعدتك 😊\n\nأنا هنا في أي وقت تحتاجني - سواء في الأوقات العادية أو في الطوارئ.\n\nتذكر: أعمل بالكامل بدون إنترنت، لذا يمكنك الاعتماد عليّ حتى في أصعب الظروف.\n\nهل هناك شيء آخر يمكنني مساعدتك به؟', suggestions: ['اسأل سؤالاً جديداً', 'وضع الطوارئ', 'نصائح يومية'] },
    { keywords: ['سكر', 'diabetes', 'سكري', 'insulin', 'أنسولين', 'glucose', 'جلوكوز'], category: 'health', response: '🍬 **معلومات عن السكري:**\n\n**أنواع السكري:**\n• **النوع الأول**: الجهاز المناعي يهاجم خلايا البنكرياس\n• **النوع الثاني**: مقاومة الأنسولين (الأكثر شيوعاً)\n• **سكري الحمل**: مؤقت أثناء الحمل\n\n**القيم الطبيعية للسكر:**\n• صائم: أقل من 100 مغ/ديسيلتر\n• بعد ساعتين من الأكل: أقل من 140 مغ/ديسيلتر\n\n**الأعراض:**\n• عطش وتبول متكرران\n• تعب وإرهاق\n• تشوش الرؤية\n• بطء شفاء الجروح\n\n**إدارة السكري:**\n• مراقبة السكر بانتظام\n• النظام الغذائي المنضبط\n• الرياضة المنتظمة\n• الأدوية/الأنسولين حسب الوصفة', suggestions: ['ضغط الدم', 'الكوليسترول', 'نصائح غذائية'] },
    { keywords: ['فيروس', 'بكتيريا', 'عدوى', 'infection', 'virus', 'bacteria', 'مناعة', 'immunity', 'كورونا', 'covid'], category: 'health', response: '🦠 **الأمراض المعدية والمناعة:**\n\n**الفرق بين الفيروسات والبكتيريا:**\n• **الفيروسات**: أصغر من البكتيريا، تتكاثر داخل الخلايا، لا تستجيب للمضادات الحيوية (مثال: الإنفلونزا، كوفيد)\n• **البكتيريا**: كائنات حية مستقلة، كثير منها مفيد، تعالج بالمضادات الحيوية عند الحاجة\n\n**تقوية المناعة:**\n1. نوم كافٍ وجيد\n2. تغذية متوازنة غنية بالفيتامينات\n3. رياضة منتظمة\n4. تجنب التوتر المزمن\n5. التطعيمات الدورية\n\n**الوقاية من العدوى:**\n• غسل اليدين بانتظام (20 ثانية)\n• تجنب لمس الوجه\n• التهوية الجيدة للأماكن', suggestions: ['الصحة العامة', 'المضادات الحيوية', 'الوقاية من الأمراض'] },
    { keywords: ['مطبخ', 'طبخ', 'وصفة', 'recipe', 'cooking', 'food preparation', 'خبز', 'baking'], category: 'lifestyle', response: '👨‍🍳 **نصائح المطبخ والطبخ:**\n\n**مهارات أساسية:**\n• تقطيع الخضروات بأمان: أبعد أصابعك، استخدم سكيناً حاداً\n• قياس المكونات بدقة خاصة في الخبز\n• درجات حرارة الطبخ: 165°م للدواجن، 145°م للحوم\n\n**الحفاظ على الطعام:**\n• ثلاجة: 1-4 درجات مئوية\n• مجمد: -18 درجة أو أقل\n• لا تترك الطعام في درجة الغرفة أكثر من ساعتين\n\n**استبدالات مفيدة:**\n• بدلاً من الزبدة: زيت نباتي أو زيت جوز الهند\n• بدلاً من البيض: موز مهروس أو كتان مطحون\n• بدلاً من اللبن الرائب: حليب + ملعقة خل\n\n**خطأ شائع:**\n• فتح الفرن كثيراً عند الخبز يخرب النتيجة!', suggestions: ['التغذية الصحية', 'حفظ الطعام', 'وصفات صحية'] },
    { keywords: ['مال', 'ادخار', 'money', 'saving', 'finance', 'budget', 'ميزانية', 'استثمار', 'investment'], category: 'lifestyle', response: '💰 **إدارة المال والادخار:**\n\n**قاعدة 50-30-20:**\n• 50%: الضروريات (إيجار، طعام، مواصلات)\n• 30%: الرغبات والترفيه\n• 20%: الادخار والاستثمار\n\n**نصائح الادخار:**\n1. افتح حساب ادخار منفصل\n2. اضبط تحويلاً تلقائياً لحساب الادخار\n3. سجّل كل مصروف لمدة شهر\n4. تخلص من الاشتراكات غير المستخدمة\n5. اشترِ بالجملة للمنتجات الأساسية\n\n**الاستثمار المبكر:**\n• ابدأ مبكراً حتى لو بمبالغ صغيرة\n• التنويع يقلل المخاطر\n• الصناديق المشتركة للمبتدئين\n• لا تستثمر مالاً لا تستطيع خسارته\n\n**الديون:**\n• سدد ديون الفائدة العالية أولاً\n• تجنب الديون للكماليات', suggestions: ['الادخار الطارئ', 'التخطيط المالي', 'الاستثمار'] },
    { keywords: ['سفر', 'travel', 'سياحة', 'tourism', 'رحلة', 'trip', 'vacation', 'بلد', 'country'], category: 'lifestyle', response: '✈️ **نصائح السفر والسياحة:**\n\n**قبل السفر:**\n• تحقق من صلاحية جواز السفر (6 أشهر على الأقل)\n• احجز التأشيرة مبكراً إذا لزم\n• خذ نسخاً من وثائقك المهمة\n• تأمين سفر\n• اطلع على ثقافة وعادات البلد\n\n**أثناء السفر:**\n• احتفظ بمحفظة احتياطية\n• شارك موقعك مع شخص موثوق\n• تعلم كلمات أساسية باللغة المحلية\n• احفظ رقم سفارة بلدك\n\n**في حالات الطوارئ بالخارج:**\n• اتصل بسفارة بلدك\n• احتفظ برقم طوارئ محلي (112 في أوروبا)\n• فندق مكان جيد للاستئناس في الأزمات\n\n**أفضل الوجهات السياحية:**\n• الأهرامات، الأردن، تركيا، إسبانيا، اليابان', suggestions: ['الحروف المصطلح الدولية', 'الطقس', 'الأمان في السفر'] }
  ];

  var EMERGENCY_GUIDES = [
    {
      id: 'health',
      title: 'طوارئ طبية',
      icon: 'favorite',
      color: '#E53935',
      description: 'نوبات قلبية، إغماء، حوادث طبية',
      steps: [
        { step: 1, title: 'تقييم الموقف', detail: 'تحقق من أمان المنطقة وتأكد أن المصاب في خطر' },
        { step: 2, title: 'اتصل بالإسعاف', detail: 'اتصل برقم الطوارئ المحلي فوراً وأخبرهم بالحالة والموقع' },
        { step: 3, title: 'لا تحرك المصاب', detail: 'إلا إذا كان في خطر داهم. خاصة في حوادث الرأس والرقبة' },
        { step: 4, title: 'الإنعاش القلبي', detail: 'إذا لم يكن هناك نبض: 30 ضغطة صدر ثم نفسين اصطناعيين، كرر حتى يصل الإسعاف' },
        { step: 5, title: 'السكتة الدماغية', detail: 'F-A-S-T: تعرج الوجه، ضعف الذراع، صعوبة الكلام، الوقت عامل حاسم' },
        { step: 6, title: 'الاختناق', detail: 'طبق مناورة هيملك: ضغطات سريعة للأعلى تحت الصدر حتى يخرج الجسم الغريب' }
      ],
      contacts: ['911 - الطوارئ العامة', '9998 - الدفاع المدني والإسعاف', '9999 - الشرطة', '113 - الشرطة العمانية']
    },
    {
      id: 'safety',
      title: 'طوارئ أمنية',
      icon: 'security',
      color: '#F57C00',
      description: 'سطو، اعتداء، خطر أمني',
      steps: [
        { step: 1, title: 'ابتعد عن الخطر', detail: 'انسحب بهدوء وتوجه لمكان مكتظ وآمن' },
        { step: 2, title: 'اتصل بالشرطة', detail: 'اتصل برقم الشرطة المحلي وأعطِ موقعك الدقيق وصف الموقف' },
        { step: 3, title: 'لا تواجه المعتدي', detail: 'سلامتك الجسدية أهم من أي ممتلكات. تعاون إذا لزم الأمر' },
        { step: 4, title: 'الاحتجاز', detail: 'إذا احتُجزت: لا تقاوم، تذكر التفاصيل (المظهر، السيارة، الاتجاه)' },
        { step: 5, title: 'بعد الحادثة', detail: 'لا تلمس أي دليل، وثّق كل شيء وأبلغ الشرطة فوراً' },
        { step: 6, title: 'الوقاية', detail: 'كن يقظاً دائماً في الأماكن العامة، تجنب المناطق المظلمة وحيداً' }
      ],
      contacts: ['911 - الطوارئ', '9999 - الشرطة العمانية', '113 - الشرطة المحلية', 'مركز الشرطة الأقرب']
    },
    {
      id: 'natural',
      title: 'كوارث طبيعية',
      icon: 'terrain',
      color: '#5E35B1',
      description: 'زلازل، فيضانات، أعاصير',
      steps: [
        { step: 1, title: 'الزلزال - أثناءه', detail: 'اجلس تحت طاولة متينة أو بجانب جدار داخلي. احمِ رأسك وعنقك بذراعيك' },
        { step: 2, title: 'الزلزال - بعده', detail: 'توقع هزات ارتدادية، تفقد للغاز والكهرباء، لا تدخل المباني المتضررة' },
        { step: 3, title: 'الفيضانات', detail: 'لا تقود في المياه الجارية. ارتفع لأعلى طابق. لا تلمس الأسلاك الكهربائية' },
        { step: 4, title: 'الأعاصير والعواصف', detail: 'ابق في الداخل، ابتعد عن النوافذ، اذهب لغرفة داخلية في الطابق الأرضي' },
        { step: 5, title: 'أعاصير الرمل', detail: 'ابق في الداخل، أغلق النوافذ والأبواب، استخدم كمامة إذا اضطررت للخروج' },
        { step: 6, title: 'بعد الكارثة', detail: 'استمع لتعليمات السلطات. تواصل مع الأسرة. ابحث عن مراكز الإيواء' }
      ],
      contacts: ['911 - الطوارئ', '9998 - الدفاع المدني', '1300 - خدمات الطوارئ', 'الدفاع المدني العماني']
    },
    {
      id: 'fire',
      title: 'حريق',
      icon: 'local-fire-department',
      color: '#FF5722',
      description: 'حريق في المنزل أو المكان',
      steps: [
        { step: 1, title: 'أعلن عن الحريق', detail: 'صِح "حريق!" لتنبيه الجميع. اضغط جرس الإنذار إذا توفر' },
        { step: 2, title: 'أخلِ المكان فوراً', detail: 'لا تأخذ أشياء. اخرج فوراً عبر السلالم لا المصاعد' },
        { step: 3, title: 'اغلق الأبواب', detail: 'أغلق الأبواب خلفك لإبطاء انتشار النار والدخان' },
        { step: 4, title: 'تفادى الدخان', detail: 'انحنِ قريباً من الأرض حيث الهواء أنظف. غطِّ أنفك بقماش رطب' },
        { step: 5, title: 'اتصل بالإطفاء', detail: 'اتصل فوراً بعد الخروج. أعطِ الموقع الدقيق وطبيعة الحريق' },
        { step: 6, title: 'لا تعد للداخل', detail: 'مهما نسيت! انتظر الإطفاء خارجاً في نقطة التجمع المحددة مسبقاً' }
      ],
      contacts: ['911 - الطوارئ', '9997 - الحماية المدنية والدفاع', '1301 - الحريق العماني', 'الدفاع المدني']
    },
    {
      id: 'accident',
      title: 'حوادث السيارات',
      icon: 'directions-car',
      color: '#00796B',
      description: 'حوادث طرق واصطدامات',
      steps: [
        { step: 1, title: 'قيّم الموقف', detail: 'تفقد نفسك أولاً، ثم الركاب، ثم الآخرين. لا تتحرك إذا كنت مصاباً' },
        { step: 2, title: 'أمّن المكان', detail: 'ضع مثلثات التحذير. أشغّل أضواء الطوارئ. أوقف المحرك' },
        { step: 3, title: 'اتصل بالطوارئ', detail: 'اتصل بالإسعاف والشرطة. أخبر بعدد المصابين وطبيعة الإصابات' },
        { step: 4, title: 'تعامل مع المصابين', detail: 'لا تحرك المصاب إلا في حالة خطر. أوقف النزيف. ادفئ المصاب' },
        { step: 5, title: 'وثّق الحادث', detail: 'التقط صوراً للحادث والأضرار. سجّل بيانات السائقين والشهود' },
        { step: 6, title: 'تجنب المواجهة', detail: 'لا تتشاجر في موقع الحادث. احتفظ بهدوئك وانتظر الشرطة' }
      ],
      contacts: ['911 - الطوارئ', '9999 - شرطة المرور', '1300 - خدمات الطوارئ', 'مركز شرطة المرور']
    }
  ];
  // @end:knowledge-base

  // @section:ai-engine @depends:[knowledge-base]
  var generateAIResponse = function(userMessage, conversationHistory) {
    var lowerMsg = userMessage.toLowerCase();
    var bestMatch = null;
    var bestScore = 0;
    for (var i = 0; i < KNOWLEDGE_BASE.length; i++) {
      var entry = KNOWLEDGE_BASE[i];
      var score = 0;
      for (var j = 0; j < entry.keywords.length; j++) {
        var keyword = entry.keywords[j].toLowerCase();
        if (lowerMsg.indexOf(keyword) !== -1) {
          score += keyword.length > 4 ? 3 : 1;
        }
      }
      if (score > bestScore) {
        bestScore = score;
        bestMatch = entry;
      }
    }
    if (bestMatch && bestScore > 0) {
      return { response: bestMatch.response, suggestions: bestMatch.suggestions || [], category: bestMatch.category };
    }
    var hasArabic = /[\u0600-\u06FF]/.test(userMessage);
    if (lowerMsg.indexOf('?') !== -1 || hasArabic) {
      var generalResponse = 'شكراً على سؤالك!\n\nهذا الموضوع يحتاج إلى معلومات أكثر تخصصاً. يمكنني مساعدتك في:\n\n🏥 **الصحة والطب**: أعراض، إسعافات، نصائح صحية\n📚 **التعليم**: علوم، رياضيات، تاريخ، جغرافيا\n💻 **التقنية**: برمجة، ذكاء اصطناعي، تكنولوجيا\n🚨 **الطوارئ**: إرشادات فورية لأي أزمة\n💡 **الحياة**: نصائح، إدارة الوقت، المال، السفر\n\nحاول إعادة صياغة سؤالك أو اختر أحد المواضيع أعلاه وسأجيبك بالتفصيل!';
      return { response: generalResponse, suggestions: ['صحة وطب', 'علوم وتعليم', 'وضع الطوارئ'], category: 'general' };
    }
    return { response: 'يمكنني مساعدتك في الصحة، التعليم، التقنية، الطوارئ، ونصائح الحياة. ما الذي تريد معرفته؟', suggestions: ['نصائح صحية', 'معلومات علمية', 'طوارئ'], category: 'general' };
  };

  var generateConversationTitle = function(firstMessage) {
    if (!firstMessage) return 'محادثة جديدة';
    var words = firstMessage.trim().split(' ');
    var title = words.slice(0, 5).join(' ');
    return title.length > 40 ? title.substring(0, 40) + '...' : title;
  };

  var generateId = function() {
    var chars = '0123456789abcdef';
    var result = '';
    var groups = [8, 4, 4, 4, 12];
    for (var g = 0; g < groups.length; g++) {
      if (g > 0) result += '-';
      for (var c = 0; c < groups[g]; c++) {
        result += chars[Math.floor(Math.random() * chars.length)];
      }
    }
    return result;
  };
  // @end:ai-engine

  // @section:navigation-setup @depends:[]
  var Tab = createBottomTabNavigator();
  // @end:navigation-setup

  // @section:ThemeContext @depends:[theme]
  var ThemeContext = React.createContext({
    theme: {
      colors: {
        primary: primaryColor,
        accent: accentColor,
        background: backgroundColor,
        card: cardColor,
        surface: surfaceColor,
        text: textPrimary,
        textSecondary: textSecondary,
        border: borderColor,
        emergency: emergencyColor,
        success: successColor,
        warning: warningColor
      }
    }
  });

  var ThemeProvider = function(props) {
    var value = useMemo(function() {
      return {
        theme: {
          colors: {
            primary: primaryColor,
            accent: accentColor,
            background: backgroundColor,
            card: cardColor,
            surface: surfaceColor,
            text: textPrimary,
            textSecondary: textSecondary,
            border: borderColor,
            emergency: emergencyColor,
            success: successColor,
            warning: warningColor
          }
        }
      };
    }, []);
    return React.createElement(ThemeContext.Provider, { value: value }, props.children);
  };

  var useTheme = function() { return useContext(ThemeContext); };
  // @end:ThemeContext

  // @section:ChatScreen-state @depends:[ThemeContext]
  var useChatState = function() {
    var themeCtx = useTheme();
    var activeConvIdState = useState(null);
    var activeConvId = activeConvIdState[0];
    var setActiveConvId = activeConvIdState[1];
    var messagesState = useState([]);
    var messages = messagesState[0];
    var setMessages = messagesState[1];
    var inputState = useState('');
    var inputText = inputState[0];
    var setInputText = inputState[1];
    var loadingState = useState(false);
    var isLoading = loadingState[0];
    var setIsLoading = loadingState[1];
    var suggestionsState = useState(['مرحبا، كيف تعمل؟', 'وضع الطوارئ', 'ما قدراتك؟']);
    var suggestions = suggestionsState[0];
    var setSuggestions = suggestionsState[1];
    return {
      theme: themeCtx.theme,
      activeConvId: activeConvId,
      setActiveConvId: setActiveConvId,
      messages: messages,
      setMessages: setMessages,
      inputText: inputText,
      setInputText: setInputText,
      isLoading: isLoading,
      setIsLoading: setIsLoading,
      suggestions: suggestions,
      setSuggestions: setSuggestions
    };
  };
  // @end:ChatScreen-state

  // @section:ChatScreen-handlers @depends:[ChatScreen-state,ai-engine]
  var createChatHandlers = function(state, insertConversation, insertMessage, refetchConversations) {
    var handleSend = function(text) {
      var msg = text !== undefined ? text : state.inputText;
      if (!msg || !msg.trim()) return;
      var trimmedMsg = msg.trim();
      state.setInputText('');
      state.setIsLoading(true);
      var userMsgId = generateId();
      var userMsg = { id: userMsgId, role: 'user', content: trimmedMsg, is_pinned: false, timestamp: new Date().toISOString() };
      var newMessages = state.messages.concat([userMsg]);
      state.setMessages(newMessages);
      var doSendWithConv = function(convId) {
        if (insertMessage) {
          insertMessage({ id: userMsgId, conversation_id: convId, role: 'user', content: trimmedMsg, is_pinned: false });
        }
        setTimeout(function() {
          var aiResult = generateAIResponse(trimmedMsg, newMessages);
          var aiMsgId = generateId();
          var aiMsg = { id: aiMsgId, role: 'assistant', content: aiResult.response, is_pinned: false, timestamp: new Date().toISOString() };
          state.setMessages(function(prev) { return prev.concat([aiMsg]); });
          state.setSuggestions(aiResult.suggestions || []);
          state.setIsLoading(false);
          if (insertMessage) {
            insertMessage({ id: aiMsgId, conversation_id: convId, role: 'assistant', content: aiResult.response, is_pinned: false });
          }
          if (refetchConversations) refetchConversations();
        }, 600);
      };
      if (!state.activeConvId) {
        var newConvId = generateId();
        state.setActiveConvId(newConvId);
        var title = generateConversationTitle(trimmedMsg);
        if (insertConversation) {
          insertConversation({ id: newConvId, user_id: 'local_user', title: title, summary: trimmedMsg.substring(0, 100) }).then(function() {
            doSendWithConv(newConvId);
          }, function() {
            doSendWithConv(newConvId);
          });
        } else {
          doSendWithConv(newConvId);
        }
      } else {
        doSendWithConv(state.activeConvId);
      }
    };
    var handleNewConversation = function() {
      state.setActiveConvId(null);
      state.setMessages([]);
      state.setSuggestions(['مرحبا، كيف تعمل؟', 'وضع الطوارئ', 'ما قدراتك؟']);
    };
    return { handleSend: handleSend, handleNewConversation: handleNewConversation };
  };
  // @end:ChatScreen-handlers

  // @section:ChatScreen-MessageBubble @depends:[styles]
  var MessageBubble = function(props) {
    var msg = props.msg;
    var theme = props.theme;
    var isUser = msg.role === 'user';
    return React.createElement(View, {
      style: { flexDirection: 'row', justifyContent: isUser ? 'flex-end' : 'flex-start', marginVertical: 4, paddingHorizontal: 12 },
      componentId: 'bubble-container-' + msg.id
    },
      !isUser && React.createElement(View, {
        style: { width: 32, height: 32, borderRadius: 16, backgroundColor: accentColor, alignItems: 'center', justifyContent: 'center', marginRight: 8, marginTop: 4, flexShrink: 0 },
        componentId: 'ai-avatar-' + msg.id
      },
        React.createElement(Text, { style: { color: '#fff', fontSize: 14, fontWeight: 'bold' } }, 'G')
      ),
      React.createElement(View, {
        style: {
          maxWidth: '80%',
          backgroundColor: isUser ? primaryColor : cardColor,
          borderRadius: isUser ? 18 : 18,
          borderBottomRightRadius: isUser ? 4 : 18,
          borderBottomLeftRadius: isUser ? 18 : 4,
          paddingHorizontal: 14,
          paddingVertical: 10,
          borderWidth: isUser ? 0 : 1,
          borderColor: borderColor
        },
        componentId: 'bubble-' + msg.id
      },
        React.createElement(Text, {
          style: { color: textPrimary, fontSize: 15, lineHeight: 22 },
          componentId: 'bubble-text-' + msg.id
        }, msg.content)
      ),
      isUser && React.createElement(View, {
        style: { width: 32, height: 32, borderRadius: 16, backgroundColor: surfaceColor, alignItems: 'center', justifyContent: 'center', marginLeft: 8, marginTop: 4, flexShrink: 0 },
        componentId: 'user-avatar-' + msg.id
      },
        React.createElement(MaterialIcons, { name: 'person', size: 18, color: textSecondary })
      )
    );
  };
  // @end:ChatScreen-MessageBubble

  // @section:ChatScreen @depends:[ChatScreen-state,ChatScreen-handlers,ChatScreen-MessageBubble,styles]
  var ChatScreen = function(props) {
    var navigation = props.navigation;
    var state = useChatState();
    var insets = useSafeAreaInsets();
    var flatListRef = useRef(null);
    var insertConvResult = useMutation('conversations', 'insert');
    var insertConv = insertConvResult.mutate;
    var insertMsgResult = useMutation('messages', 'insert');
    var insertMsg = insertMsgResult.mutate;
    var convsQuery = useQuery('conversations', { user_id: 'local_user' }, { column: 'created_at', ascending: false });
    var refetchConvs = convsQuery.refetch;
    var handlers = createChatHandlers(state, insertConv, insertMsg, refetchConvs);
    var scrollBottomPadding = Platform.OS === 'web' ? WEB_TAB_MENU_PADDING : (TAB_MENU_HEIGHT + insets.bottom + SCROLL_EXTRA_PADDING);
    useEffect(function() {
      if (flatListRef.current && state.messages.length > 0) {
        setTimeout(function() {
          if (flatListRef.current) {
            flatListRef.current.scrollToEnd({ animated: true });
          }
        }, 100);
      }
    }, [state.messages.length]);
    var renderSuggestion = function(sugg, index) {
      return React.createElement(TouchableOpacity, {
        key: String(index),
        onPress: function() { handlers.handleSend(sugg); },
        style: { backgroundColor: surfaceColor, borderColor: accentColor, borderWidth: 1, borderRadius: 20, paddingHorizontal: 14, paddingVertical: 7, marginRight: 8, marginBottom: 6 },
        componentId: 'suggestion-' + index
      },
        React.createElement(Text, { style: { color: accentColor, fontSize: 13 } }, sugg)
      );
    };
    var showWelcome = state.messages.length === 0;
    return React.createElement(View, { style: { flex: 1, backgroundColor: backgroundColor }, componentId: 'chat-screen' },
      React.createElement(View, {
        style: { backgroundColor: cardColor, paddingTop: insets.top + 8, paddingBottom: 12, paddingHorizontal: 16, flexDirection: 'row', alignItems: 'center', borderBottomWidth: 1, borderBottomColor: borderColor },
        componentId: 'chat-header'
      },
        React.createElement(View, { style: { width: 38, height: 38, borderRadius: 19, backgroundColor: accentColor, alignItems: 'center', justifyContent: 'center', marginRight: 10 }, componentId: 'header-logo' },
          React.createElement(Text, { style: { color: '#fff', fontWeight: 'bold', fontSize: 16 } }, 'G')
        ),
        React.createElement(View, { style: { flex: 1 }, componentId: 'header-info' },
          React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 17 }, componentId: 'header-title' }, 'ChatGLK'),
          React.createElement(View, { style: { flexDirection: 'row', alignItems: 'center' }, componentId: 'header-status' },
            React.createElement(View, { style: { width: 7, height: 7, borderRadius: 4, backgroundColor: successColor, marginRight: 5 }, componentId: 'status-dot' }),
            React.createElement(Text, { style: { color: successColor, fontSize: 12 }, componentId: 'status-text' }, 'يعمل بدون إنترنت')
          )
        ),
        React.createElement(TouchableOpacity, {
          onPress: handlers.handleNewConversation,
          style: { padding: 8 },
          componentId: 'new-chat-btn'
        },
          React.createElement(MaterialIcons, { name: 'add-comment', size: 22, color: accentColor })
        )
      ),
      showWelcome
        ? React.createElement(ScrollView, {
            style: { flex: 1 },
            contentContainerStyle: { padding: 20, paddingBottom: scrollBottomPadding, alignItems: 'center' },
            componentId: 'welcome-scroll'
          },
            React.createElement(View, { style: { width: 80, height: 80, borderRadius: 40, backgroundColor: primaryColor, alignItems: 'center', justifyContent: 'center', marginTop: 20, marginBottom: 16, borderWidth: 3, borderColor: accentColor }, componentId: 'welcome-logo' },
              React.createElement(Text, { style: { color: accentColor, fontSize: 32, fontWeight: 'bold' } }, 'G')
            ),
            React.createElement(Text, { style: { color: textPrimary, fontSize: 22, fontWeight: 'bold', marginBottom: 8, textAlign: 'center' }, componentId: 'welcome-title' }, 'مرحباً بك في ChatGLK'),
            React.createElement(Text, { style: { color: textSecondary, fontSize: 15, textAlign: 'center', lineHeight: 22, marginBottom: 24 }, componentId: 'welcome-subtitle' }, 'مساعد الذكاء الاصطناعي المحلي\nيعمل بالكامل بدون إنترنت'),
            React.createElement(View, { style: { flexDirection: 'row', flexWrap: 'wrap', justifyContent: 'center', marginBottom: 20 }, componentId: 'welcome-badges' },
              React.createElement(View, { style: { backgroundColor: surfaceColor, borderRadius: 12, paddingHorizontal: 12, paddingVertical: 6, margin: 4, flexDirection: 'row', alignItems: 'center' }, componentId: 'badge-offline' },
                React.createElement(MaterialIcons, { name: 'wifi-off', size: 14, color: successColor }),
                React.createElement(Text, { style: { color: successColor, fontSize: 12, marginLeft: 4 } }, 'بدون إنترنت')
              ),
              React.createElement(View, { style: { backgroundColor: surfaceColor, borderRadius: 12, paddingHorizontal: 12, paddingVertical: 6, margin: 4, flexDirection: 'row', alignItems: 'center' }, componentId: 'badge-free' },
                React.createElement(MaterialIcons, { name: 'star', size: 14, color: warningColor }),
                React.createElement(Text, { style: { color: warningColor, fontSize: 12, marginLeft: 4 } }, 'مجاني 100%')
              ),
              React.createElement(View, { style: { backgroundColor: surfaceColor, borderRadius: 12, paddingHorizontal: 12, paddingVertical: 6, margin: 4, flexDirection: 'row', alignItems: 'center' }, componentId: 'badge-secure' },
                React.createElement(MaterialIcons, { name: 'lock', size: 14, color: accentColor }),
                React.createElement(Text, { style: { color: accentColor, fontSize: 12, marginLeft: 4 } }, 'خاص وآمن')
              )
            ),
            React.createElement(Text, { style: { color: textSecondary, fontSize: 14, marginBottom: 14, textAlign: 'center' }, componentId: 'suggestions-label' }, 'ابدأ بسؤال:'),
            React.createElement(View, { style: { flexDirection: 'row', flexWrap: 'wrap', justifyContent: 'center' }, componentId: 'welcome-suggestions' },
              state.suggestions.map(function(sugg, i) { return renderSuggestion(sugg, i); })
            )
          )
        : React.createElement(FlatList, {
            ref: flatListRef,
            data: state.messages,
            keyExtractor: function(item) { return item.id; },
            renderItem: function(itemData) { return React.createElement(MessageBubble, { msg: itemData.item, theme: state.theme }); },
            contentContainerStyle: { paddingTop: 12, paddingBottom: scrollBottomPadding + 80 },
            style: { flex: 1 },
            componentId: 'messages-list'
          }),
      state.isLoading && React.createElement(View, { style: { paddingHorizontal: 16, paddingBottom: 8, flexDirection: 'row', alignItems: 'center' }, componentId: 'typing-indicator' },
        React.createElement(View, { style: { width: 32, height: 32, borderRadius: 16, backgroundColor: accentColor, alignItems: 'center', justifyContent: 'center', marginRight: 8 }, componentId: 'typing-avatar' },
          React.createElement(Text, { style: { color: '#fff', fontSize: 14, fontWeight: 'bold' } }, 'G')
        ),
        React.createElement(View, { style: { backgroundColor: cardColor, borderRadius: 18, borderBottomLeftRadius: 4, paddingHorizontal: 14, paddingVertical: 10, borderWidth: 1, borderColor: borderColor }, componentId: 'typing-bubble' },
          React.createElement(ActivityIndicator, { size: 'small', color: accentColor })
        )
      ),
      !showWelcome && state.suggestions.length > 0 && !state.isLoading && React.createElement(ScrollView, {
        horizontal: true,
        showsHorizontalScrollIndicator: false,
        style: { paddingHorizontal: 12, paddingVertical: 8, maxHeight: 60, flexGrow: 0 },
        contentContainerStyle: { alignItems: 'center' },
        componentId: 'suggestions-scroll'
      },
        state.suggestions.map(function(sugg, i) { return renderSuggestion(sugg, i); })
      ),
      React.createElement(KeyboardAvoidingView, {
        behavior: Platform.OS === 'ios' ? 'padding' : (Platform.OS === 'web' ? undefined : 'height'),
        keyboardVerticalOffset: Platform.OS === 'ios' ? (TAB_MENU_HEIGHT + insets.bottom) : 0,
        componentId: 'keyboard-avoid'
      },
        React.createElement(View, {
          style: { flexDirection: 'row', alignItems: 'flex-end', paddingHorizontal: 12, paddingVertical: 10, paddingBottom: Platform.OS === 'web' ? 16 : (insets.bottom + 10), backgroundColor: cardColor, borderTopWidth: 1, borderTopColor: borderColor },
          componentId: 'input-bar'
        },
          React.createElement(TextInput, {
            value: state.inputText,
            onChangeText: state.setInputText,
            placeholder: 'اسألني أي شيء...',
            placeholderTextColor: textSecondary,
            multiline: true,
            numberOfLines: 3,
            textAlignVertical: 'top',
            style: { flex: 1, backgroundColor: surfaceColor, borderRadius: 20, paddingHorizontal: 16, paddingVertical: 10, color: textPrimary, fontSize: 15, maxHeight: 100, marginRight: 8, borderWidth: 1, borderColor: borderColor },
            componentId: 'message-input'
          }),
          React.createElement(TouchableOpacity, {
            onPress: function() { handlers.handleSend(state.inputText); },
            style: { width: 44, height: 44, borderRadius: 22, backgroundColor: state.inputText.trim() ? accentColor : surfaceColor, alignItems: 'center', justifyContent: 'center' },
            componentId: 'send-btn'
          },
            React.createElement(MaterialIcons, { name: 'send', size: 20, color: state.inputText.trim() ? '#fff' : textSecondary })
          )
        )
      )
    );
  };
  // @end:ChatScreen

  // @section:EmergencyScreen-CategoryCard @depends:[styles]
  var EmergencyCategoryCard = function(props) {
    var guide = props.guide;
    var onPress = props.onPress;
    return React.createElement(TouchableOpacity, {
      onPress: onPress,
      style: { backgroundColor: cardColor, borderRadius: 16, padding: 16, marginBottom: 12, flexDirection: 'row', alignItems: 'center', borderWidth: 1, borderColor: borderColor, borderLeftWidth: 4, borderLeftColor: guide.color },
      componentId: 'emergency-card-' + guide.id
    },
      React.createElement(View, { style: { width: 48, height: 48, borderRadius: 24, backgroundColor: guide.color + '22', alignItems: 'center', justifyContent: 'center', marginRight: 14 }, componentId: 'emergency-icon-bg-' + guide.id },
        React.createElement(MaterialIcons, { name: guide.icon, size: 26, color: guide.color })
      ),
      React.createElement(View, { style: { flex: 1 }, componentId: 'emergency-card-info-' + guide.id },
        React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 16, marginBottom: 3 }, componentId: 'emergency-card-title-' + guide.id }, guide.title),
        React.createElement(Text, { style: { color: textSecondary, fontSize: 13 }, componentId: 'emergency-card-desc-' + guide.id }, guide.description)
      ),
      React.createElement(MaterialIcons, { name: 'chevron-right', size: 22, color: textSecondary })
    );
  };
  // @end:EmergencyScreen-CategoryCard

  // @section:EmergencyDetail @depends:[styles]
  var EmergencyDetailModal = function(props) {
    var guide = props.guide;
    var visible = props.visible;
    var onClose = props.onClose;
    var theme = props.theme;
    var insetsTop = props.insetsTop;
    var insetsBottom = props.insetsBottom;
    var notesState = useState('');
    var notes = notesState[0];
    var setNotes = notesState[1];
    var savedState = useState(false);
    var saved = savedState[0];
    var setSaved = savedState[1];
    var insertLogResult = useMutation('emergency_logs', 'insert');
    var insertLog = insertLogResult.mutate;
    var sheetHeight = Math.round(Dimensions.get('window').height * 0.9);
    if (!guide) return null;
    var handleSave = function() {
      var logId = generateId();
      var responseText = guide.steps.map(function(s) { return s.step + '. ' + s.title + ': ' + s.detail; }).join('\n');
      insertLog({ id: logId, category: guide.title, response_given: responseText, user_notes: notes }).then(function() {
        setSaved(true);
        setTimeout(function() { setSaved(false); }, 3000);
      }, function() {
        setSaved(true);
        setTimeout(function() { setSaved(false); }, 3000);
      });
    };
    return React.createElement(Modal, {
      visible: visible,
      animationType: 'slide',
      transparent: true,
      onRequestClose: onClose
    },
      React.createElement(View, { style: { flex: 1, justifyContent: 'flex-end', backgroundColor: 'rgba(0,0,0,0.7)' }, componentId: 'emergency-modal-overlay' },
        React.createElement(View, { style: { height: sheetHeight, backgroundColor: backgroundColor, borderTopLeftRadius: 20, borderTopRightRadius: 20 }, componentId: 'emergency-modal-content' },
          React.createElement(View, { style: { flexDirection: 'row', alignItems: 'center', padding: 16, borderBottomWidth: 1, borderBottomColor: borderColor, backgroundColor: cardColor, borderTopLeftRadius: 20, borderTopRightRadius: 20 }, componentId: 'emergency-modal-header' },
            React.createElement(View, { style: { width: 40, height: 40, borderRadius: 20, backgroundColor: guide.color + '33', alignItems: 'center', justifyContent: 'center', marginRight: 12 }, componentId: 'modal-icon-bg' },
              React.createElement(MaterialIcons, { name: guide.icon, size: 22, color: guide.color })
            ),
            React.createElement(Text, { style: { flex: 1, color: textPrimary, fontWeight: 'bold', fontSize: 18 }, componentId: 'modal-title' }, guide.title),
            React.createElement(TouchableOpacity, { onPress: onClose, style: { padding: 8 }, componentId: 'modal-close-btn' },
              React.createElement(MaterialIcons, { name: 'close', size: 24, color: textSecondary })
            )
          ),
          React.createElement(ScrollView, { style: { flex: 1 }, contentContainerStyle: { padding: 16, paddingBottom: insetsBottom + 20 }, componentId: 'emergency-modal-scroll' },
            React.createElement(View, { style: { backgroundColor: guide.color + '15', borderRadius: 12, padding: 12, marginBottom: 16, borderWidth: 1, borderColor: guide.color + '44' }, componentId: 'sos-banner' },
              React.createElement(Text, { style: { color: guide.color, fontWeight: 'bold', fontSize: 15, textAlign: 'center' }, componentId: 'sos-text' }, '⚠️ اتبع هذه الخطوات بترتيب دقيق')
            ),
            guide.steps.map(function(step) {
              return React.createElement(View, { key: String(step.step), style: { flexDirection: 'row', marginBottom: 14 }, componentId: 'step-' + step.step },
                React.createElement(View, { style: { width: 32, height: 32, borderRadius: 16, backgroundColor: guide.color, alignItems: 'center', justifyContent: 'center', marginRight: 12, flexShrink: 0, marginTop: 2 }, componentId: 'step-num-bg-' + step.step },
                  React.createElement(Text, { style: { color: '#fff', fontWeight: 'bold', fontSize: 14 }, componentId: 'step-num-' + step.step }, String(step.step))
                ),
                React.createElement(View, { style: { flex: 1 }, componentId: 'step-info-' + step.step },
                  React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 15, marginBottom: 3 }, componentId: 'step-title-' + step.step }, step.title),
                  React.createElement(Text, { style: { color: textSecondary, fontSize: 14, lineHeight: 20 }, componentId: 'step-detail-' + step.step }, step.detail)
                )
              );
            }),
            React.createElement(View, { style: { backgroundColor: cardColor, borderRadius: 12, padding: 14, marginTop: 8, marginBottom: 12, borderWidth: 1, borderColor: borderColor }, componentId: 'contacts-section' },
              React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 15, marginBottom: 10 }, componentId: 'contacts-title' }, '📞 أرقام الطوارئ العمانية'),
              guide.contacts.map(function(contact, ci) {
                return React.createElement(View, { key: String(ci), style: { flexDirection: 'row', alignItems: 'center', marginBottom: 6 }, componentId: 'contact-' + ci },
                  React.createElement(View, { style: { width: 6, height: 6, borderRadius: 3, backgroundColor: accentColor, marginRight: 10 }, componentId: 'contact-dot-' + ci }),
                  React.createElement(Text, { style: { color: textSecondary, fontSize: 14 }, componentId: 'contact-text-' + ci }, contact)
                );
              })
            ),
            React.createElement(View, { style: { backgroundColor: cardColor, borderRadius: 12, padding: 14, marginBottom: 12, borderWidth: 1, borderColor: borderColor }, componentId: 'notes-section' },
              React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 14, marginBottom: 8 }, componentId: 'notes-label' }, 'ملاحظاتك (اختياري):'),
              React.createElement(TextInput, {
                value: notes,
                onChangeText: setNotes,
                placeholder: 'أضف ملاحظات أو تفاصيل...',
                placeholderTextColor: textSecondary,
                multiline: true,
                numberOfLines: 3,
                textAlignVertical: 'top',
                style: { backgroundColor: surfaceColor, borderRadius: 10, padding: 12, color: textPrimary, fontSize: 14, minHeight: 80, borderWidth: 1, borderColor: borderColor },
                componentId: 'notes-input'
              })
            ),
            React.createElement(TouchableOpacity, {
              onPress: handleSave,
              style: { backgroundColor: saved ? successColor : guide.color, borderRadius: 12, padding: 14, alignItems: 'center', flexDirection: 'row', justifyContent: 'center' },
              componentId: 'save-log-btn'
            },
              React.createElement(MaterialIcons, { name: saved ? 'check' : 'save', size: 20, color: '#fff' }),
              React.createElement(Text, { style: { color: '#fff', fontWeight: 'bold', fontSize: 15, marginLeft: 8 }, componentId: 'save-log-text' }, saved ? 'تم الحفظ!' : 'حفظ في السجل')
            )
          )
        )
      )
    );
  };
  // @end:EmergencyDetail

  // @section:EmergencyScreen @depends:[EmergencyScreen-CategoryCard,EmergencyDetail,styles]
  var EmergencyScreen = function(props) {
    var themeCtx = useTheme();
    var insets = useSafeAreaInsets();
    var selectedGuideState = useState(null);
    var selectedGuide = selectedGuideState[0];
    var setSelectedGuide = selectedGuideState[1];
    var scrollBottomPadding = Platform.OS === 'web' ? WEB_TAB_MENU_PADDING : (TAB_MENU_HEIGHT + insets.bottom + SCROLL_EXTRA_PADDING);
    return React.createElement(View, { style: { flex: 1, backgroundColor: backgroundColor }, componentId: 'emergency-screen' },
      React.createElement(View, {
        style: { backgroundColor: emergencyColor, paddingTop: insets.top + 8, paddingBottom: 16, paddingHorizontal: 16 },
        componentId: 'emergency-header'
      },
        React.createElement(View, { style: { flexDirection: 'row', alignItems: 'center', marginBottom: 6 }, componentId: 'emergency-header-row' },
          React.createElement(View, { style: { backgroundColor: 'rgba(255,255,255,0.2)', borderRadius: 20, padding: 6, marginRight: 10 }, componentId: 'emergency-icon-bg' },
            React.createElement(MaterialIcons, { name: 'warning', size: 22, color: '#fff' })
          ),
          React.createElement(Text, { style: { color: '#fff', fontWeight: 'bold', fontSize: 20 }, componentId: 'emergency-header-title' }, 'وضع الطوارئ - عمّان')
        ),
        React.createElement(Text, { style: { color: 'rgba(255,255,255,0.85)', fontSize: 13 }, componentId: 'emergency-header-sub' }, 'يعمل بالكامل بدون إنترنت • إرشادات فورية ودقيقة عمانية')
      ),
      React.createElement(ScrollView, {
        style: { flex: 1 },
        contentContainerStyle: { padding: 16, paddingTop: 20, paddingBottom: scrollBottomPadding },
        componentId: 'emergency-scroll'
      },
        React.createElement(View, { style: { backgroundColor: '#E5393522', borderRadius: 12, padding: 14, marginBottom: 20, borderWidth: 1, borderColor: '#E5393555' }, componentId: 'emergency-notice' },
          React.createElement(Text, { style: { color: '#FF6B6B', fontWeight: 'bold', fontSize: 14, marginBottom: 4 }, componentId: 'notice-title' }, '⚠️ ملاحظة مهمة:'),
          React.createElement(Text, { style: { color: textSecondary, fontSize: 13, lineHeight: 19 }, componentId: 'notice-text' }, 'في حالات الخطر الشديد، اتصل بالطوارئ فوراً. هذا التطبيق يقدم إرشادات تكميلية فقط.\n\n📞 أرقام الطوارئ السريعة في عمّان:\n• 911 - الطوارئ العامة\n• 9998 - الدفاع المدني والإسعاف\n• 9999 - الشرطة')
        ),
        React.createElement(Text, { style: { color: textSecondary, fontSize: 13, marginBottom: 14, fontWeight: '600', letterSpacing: 0.5 }, componentId: 'categories-label' }, 'اختر نوع الطارئ:'),
        EMERGENCY_GUIDES.map(function(guide) {
          return React.createElement(EmergencyCategoryCard, {
            key: guide.id,
            guide: guide,
            onPress: function() { setSelectedGuide(guide); }
          });
        }),
        React.createElement(View, { style: { backgroundColor: cardColor, borderRadius: 16, padding: 16, marginTop: 8, borderWidth: 1, borderColor: borderColor }, componentId: 'emergency-tip' },
          React.createElement(Text, { style: { color: accentColor, fontWeight: 'bold', fontSize: 14, marginBottom: 8 }, componentId: 'tip-title' }, '💡 نصيحة الطوارئ:'),
          React.createElement(Text, { style: { color: textSecondary, fontSize: 13, lineHeight: 20 }, componentId: 'tip-text' }, 'تذكر دائماً: سلامتك أولاً. لا تعرض نفسك للخطر لإنقاذ الممتلكات. في الطوارئ: تنفس، قيّم الموقف، ثم تصرف بهدوء.')
        )
      ),
      React.createElement(EmergencyDetailModal, {
        guide: selectedGuide,
        visible: selectedGuide !== null,
        onClose: function() { setSelectedGuide(null); },
        theme: themeCtx.theme,
        insetsTop: insets.top,
        insetsBottom: insets.bottom
      })
    );
  };
  // @end:EmergencyScreen

  // @section:HistoryScreen @depends:[styles]
  var HistoryScreen = function(props) {
    var themeCtx = useTheme();
    var insets = useSafeAreaInsets();
    var convsQuery = useQuery('conversations', { user_id: 'local_user' }, { column: 'created_at', ascending: false });
    var conversations = convsQuery.data;
    var convLoading = convsQuery.loading;
    var refetchConvs = convsQuery.refetch;
    var deleteConvResult = useMutation('conversations', 'delete');
    var deleteConv = deleteConvResult.mutate;
    var selectedConvState = useState(null);
    var selectedConv = selectedConvState[0];
    var setSelectedConv = selectedConvState[1];
    var convMsgsQuery = useQuery('messages', selectedConv ? { conversation_id: selectedConv.id } : {});
    var convMessages = convMsgsQuery.data;
    var scrollBottomPadding = Platform.OS === 'web' ? WEB_TAB_MENU_PADDING : (TAB_MENU_HEIGHT + insets.bottom + SCROLL_EXTRA_PADDING);
    var handleDelete = function(convId) {
      var doDelete = function() {
        deleteConv({ id: convId }).then(function() {
          refetchConvs();
        }, function() {
          refetchConvs();
        });
      };
      if (Platform.OS === 'web') {
        if (window.confirm('حذف هذه المحادثة؟')) doDelete();
      } else {
        Alert.alert('حذف', 'هل تريد حذف هذه المحادثة؟', [
          { text: 'إلغاء', style: 'cancel' },
          { text: 'حذف', style: 'destructive', onPress: doDelete }
        ]);
      }
    };
    var formatDate = function(dateStr) {
      if (!dateStr) return '';
      var d = new Date(dateStr);
      if (isNaN(d.getTime())) return '';
      var day = d.getDate();
      var month = d.getMonth() + 1;
      var year = d.getFullYear();
      var hours = d.getHours();
      var mins = d.getMinutes();
      return day + '/' + month + '/' + year + ' ' + hours + ':' + (mins < 10 ? '0' + mins : mins);
    };
    var sheetHeight = Math.round(Dimensions.get('window').height * 0.88);
    return React.createElement(View, { style: { flex: 1, backgroundColor: backgroundColor }, componentId: 'history-screen' },
      React.createElement(View, {
        style: { backgroundColor: cardColor, paddingTop: insets.top + 8, paddingBottom: 14, paddingHorizontal: 16, borderBottomWidth: 1, borderBottomColor: borderColor },
        componentId: 'history-header'
      },
        React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 20 }, componentId: 'history-title' }, 'سجل المحادثات'),
        React.createElement(Text, { style: { color: textSecondary, fontSize: 13, marginTop: 2 }, componentId: 'history-sub' }, 'جميع محادثاتك محفوظة محلياً على جهازك')
      ),
      convLoading
        ? React.createElement(View, { style: { flex: 1, alignItems: 'center', justifyContent: 'center' }, componentId: 'history-loading' },
            React.createElement(ActivityIndicator, { size: 'large', color: accentColor, componentId: 'history-spinner' })
          )
        : (!conversations || conversations.length === 0)
          ? React.createElement(View, { style: { flex: 1, alignItems: 'center', justifyContent: 'center', padding: 32 }, componentId: 'history-empty' },
              React.createElement(View, { style: { width: 80, height: 80, borderRadius: 40, backgroundColor: surfaceColor, alignItems: 'center', justifyContent: 'center', marginBottom: 16 }, componentId: 'empty-icon-bg' },
                React.createElement(MaterialIcons, { name: 'chat-bubble-outline', size: 36, color: textSecondary })
              ),
              React.createElement(Text, { style: { color: textSecondary, fontSize: 16, fontWeight: '600', textAlign: 'center', marginBottom: 8 }, componentId: 'empty-title' }, 'لا توجد محادثات'),
              React.createElement(Text, { style: { color: textSecondary, fontSize: 13, textAlign: 'center' }, componentId: 'empty-text' }, 'ابدأ محادثة من تبويب الدردشة\nوستظهر هنا تلقائياً')
            )
          : React.createElement(FlatList, {
              data: conversations,
              keyExtractor: function(item) { return item.id; },
              contentContainerStyle: { padding: 14, paddingBottom: scrollBottomPadding },
              componentId: 'history-list',
              renderItem: function(itemData) {
                var conv = itemData.item;
                return React.createElement(TouchableOpacity, {
                  onPress: function() { setSelectedConv(conv); },
                  style: { backgroundColor: cardColor, borderRadius: 14, padding: 14, marginBottom: 10, borderWidth: 1, borderColor: borderColor },
                  componentId: 'history-item-' + conv.id
                },
                  React.createElement(View, { style: { flexDirection: 'row', alignItems: 'flex-start', justifyContent: 'space-between' }, componentId: 'history-item-row-' + conv.id },
                    React.createElement(View, { style: { flex: 1, marginRight: 10 }, componentId: 'history-item-info-' + conv.id },
                      React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 15, marginBottom: 4 }, numberOfLines: 2, componentId: 'history-item-title-' + conv.id }, conv.title || 'محادثة بدون عنوان'),
                      conv.summary && React.createElement(Text, { style: { color: textSecondary, fontSize: 13, marginBottom: 6 }, numberOfLines: 2, componentId: 'history-item-summary-' + conv.id }, conv.summary),
                      React.createElement(View, { style: { flexDirection: 'row', alignItems: 'center' }, componentId: 'history-item-meta-' + conv.id },
                        React.createElement(MaterialIcons, { name: 'schedule', size: 12, color: textSecondary }),
                        React.createElement(Text, { style: { color: textSecondary, fontSize: 12, marginLeft: 4 }, componentId: 'history-item-date-' + conv.id }, formatDate(conv.created_at))
                      )
                    ),
                    React.createElement(TouchableOpacity, {
                      onPress: function() { handleDelete(conv.id); },
                      style: { padding: 6 },
                      componentId: 'delete-btn-' + conv.id
                    },
                      React.createElement(MaterialIcons, { name: 'delete-outline', size: 20, color: emergencyColor + 'AA' })
                    )
                  )
                );
              }
            }),
      selectedConv && React.createElement(Modal, {
        visible: selectedConv !== null,
        animationType: 'slide',
        transparent: true,
        onRequestClose: function() { setSelectedConv(null); }
      },
        React.createElement(View, { style: { flex: 1, justifyContent: 'flex-end', backgroundColor: 'rgba(0,0,0,0.7)' }, componentId: 'conv-modal-overlay' },
          React.createElement(View, { style: { height: sheetHeight, backgroundColor: backgroundColor, borderTopLeftRadius: 20, borderTopRightRadius: 20 }, componentId: 'conv-modal-content' },
            React.createElement(View, { style: { flexDirection: 'row', alignItems: 'center', padding: 16, borderBottomWidth: 1, borderBottomColor: borderColor, backgroundColor: cardColor, borderTopLeftRadius: 20, borderTopRightRadius: 20 }, componentId: 'conv-modal-header' },
              React.createElement(View, { style: { flex: 1 }, componentId: 'conv-modal-title-area' },
                React.createElement(Text, { style: { color: textPrimary, fontWeight: 'bold', fontSize: 16 }, numberOfLines: 1, componentId: 'conv-modal-title' }, selectedConv.title || 'المحادثة'),
                React.createElement(Text, { style: { color: textSecondary, fontSize: 12 }, componentId: 'conv-modal-date' }, formatDate(selectedConv.created_at))
              ),
              React.createElement(TouchableOpacity, { onPress: function() { setSelectedConv(null); }, style: { padding: 8 }, componentId: 'conv-modal-close' },
                React.createElement(MaterialIcons, { name: 'close', size: 24, color: textSecondary })
              )
            ),
            React.createElement(FlatList, {
              data: convMessages || [],
              keyExtractor: function(item) { return item.id; },
              contentContainerStyle: { padding: 14, paddingBottom: insets.bottom + 20 },
              componentId: 'conv-messages-list',
              ListEmptyComponent: React.createElement(View, { style: { padding: 32, alignItems: 'center' }, componentId: 'conv-empty' },
                React.createElement(ActivityIndicator, { size: 'small', color: accentColor }),
                React.createElement(Text, { style: { color: textSecondary, marginTop: 10 }, componentId: 'conv-loading-text' }, 'جاري التحميل...')
              ),
              renderItem: function(itemData) {
                var msg = itemData.item;
                var isUser = msg.role === 'user';
                return React.createElement(View, {
                  style: { flexDirection: 'row', justifyContent: isUser ? 'flex-end' : 'flex-start', marginBottom: 10 },
                  componentId: 'conv-msg-' + msg.id
                },
                  React.createElement(View, {
                    style: { maxWidth: '80%', backgroundColor: isUser ? primaryColor : cardColor, borderRadius: 14, borderBottomRightRadius: isUser ? 4 : 14, borderBottomLeftRadius: isUser ? 14 : 4, padding: 12, borderWidth: isUser ? 0 : 1, borderColor: borderColor },
                    componentId: 'conv-bubble-' + msg.id
                  },
                    React.createElement(Text, { style: { color: textPrimary, fontSize: 14, lineHeight: 20 }, componentId: 'conv-bubble-text-' + msg.id }, msg.content)
                  )
                );
              }
            })
          )
        )
      )
    );
  };
  // @end:HistoryScreen

  // @section:styles @depends:[theme]
  var styles = StyleSheet.create({
    container: {
      flex: 1,
      backgroundColor: backgroundColor
    },
    headerBar: {
      backgroundColor: cardColor,
      borderBottomWidth: 1,
      borderBottomColor: borderColor
    },
    card: {
      backgroundColor: cardColor,
      borderRadius: 14,
      borderWidth: 1,
      borderColor: borderColor
    },
    fab: {
      position: 'absolute',
      right: 20,
      width: 52,
      height: 52,
      borderRadius: 26,
      backgroundColor: accentColor,
      alignItems: 'center',
      justifyContent: 'center',
      shadowColor: accentColor,
      shadowOffset: { width: 0, height: 4 },
      shadowOpacity: 0.4,
      shadowRadius: 8,
      elevation: 8
    },
    tabBar: {
      backgroundColor: cardColor,
      borderTopWidth: 1,
      borderTopColor: borderColor
    }
  });
  // @end:styles

  // @section:TabNavigator @depends:[ChatScreen,EmergencyScreen,HistoryScreen,navigation-setup,styles]
  var TabNavigator = function() {
    var insets = useSafeAreaInsets();
    var themeCtx = useTheme();
    return React.createElement(View, { style: { flex: 1, width: '100%', height: '100%', overflow: 'hidden' }, componentId: 'tab-wrapper' },
      React.createElement(Tab.Navigator, {
        screenOptions: {
          headerShown: false,
          tabBarStyle: {
            position: 'absolute',
            bottom: 0,
            height: Platform.OS === 'web' ? TAB_MENU_HEIGHT : TAB_MENU_HEIGHT + insets.bottom,
            paddingBottom: 0,
            borderTopWidth: 1,
            borderTopColor: borderColor,
            backgroundColor: cardColor
          },
          tabBarItemStyle: { padding: 0 },
          tabBarActiveTintColor: accentColor,
          tabBarInactiveTintColor: textSecondary
        }
      },
        React.createElement(Tab.Screen, {
          name: 'Chat',
          component: ChatScreen,
          options: {
            tabBarLabel: 'الدردشة',
            tabBarIcon: function(iconProps) { return React.createElement(MaterialIcons, { name: 'chat', size: 24, color: iconProps.color }); }
          }
        }),
        React.createElement(Tab.Screen, {
          name: 'Emergency',
          component: EmergencyScreen,
          options: {
            tabBarLabel: 'الطوارئ',
            tabBarIcon: function(iconProps) { return React.createElement(MaterialIcons, { name: 'warning', size: 24, color: iconProps.color }); }
          }
        }),
        React.createElement(Tab.Screen, {
          name: 'History',
          component: HistoryScreen,
          options: {
            tabBarLabel: 'السجل',
            tabBarIcon: function(iconProps) { return React.createElement(MaterialIcons, { name: 'history', size: 24, color: iconProps.color }); }
          }
        })
      )
    );
  };
  // @end:TabNavigator

  // @section:return @depends:[ThemeProvider,TabNavigator]
  return React.createElement(ThemeProvider, null,
    React.createElement(View, { style: { flex: 1, width: '100%', height: '100%' }, componentId: 'root-view' },
      React.createElement(StatusBar, { barStyle: 'light-content', backgroundColor: cardColor }),
      React.createElement(TabNavigator)
    )
  );
  // @end:return
};
return ComponentFunctio
