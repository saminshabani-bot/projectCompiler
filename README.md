این پروژه برای مبهم سازی زیر برنامه ها به زبان cاست.
با استفاده از زبان پایتون و antlr4 نوشته شده است.
شامل یک گرامر MiniC.g4  است که با استفاده از antlr ران شده و فایل های خروجی کد نظر که شامل lexer و parser است را تولید میکند.
بخش مبهم سازی شامل 3 فایل پایتون است.
mian.py generator.py obfuscator.py

عملیات  MiniC  با استفاده از قواعد گرامری Obfuscation  ابتدا در کلاس 
های زیر را ایجاد می کنیم :
class ObfuscatorVisitor(MiniCVisitor):
    def __init__(self):
        self.var_counter = 1
        self.func_counter = 1
        self.var_map = {}
        self.func_map = {}
        self.dead_counter = 1
در این قسمت شمارنده ها را ذخیره می کنیم برای اینکه در تولید نام های جدید متغیر ها و توابع از آنها استفاده کنیم . در واقع 
 برای نگاشت نام های اصلی به نام های مبهم شدهFunc_map و  var_map  
استفاده می شود .
 اگر نام تابع مون مین باشد به یک کنترل تخت تبدیل Visitfunction در قسمت 
می شود و در غیر این صورت هیچ تغیری نمی کند .
در قسمت های بعد تغیر نام متغیر ها و توابع را پیاده سازی می کنیم . همچنین کد های مرده و ادغام توابع و استفاده از معادلات پیچیده تر و درهم ریختگی ساختار کنترلی را ایجاد می کنیم . 
در واقع در این کلاس به تعریف عملیات هایی که باید بر روی کد اصلی ما انجام شود پرداخته ایم . 

 با استفاده از درخت نحوی کد تغیر یافته مون میایم و Generator در کلاس
یک فایل متنی از کد مون درست می کنیم که در واقع اون تغیرات ایجاد شده در کد را به ما نشان می دهد .
def visitProgram(self, ctx: MiniCParser.ProgramContext):
    return '\n\n'.join([self.visit(child) for child in ctx.children[:-1]])
این قسمت در واقع تابع اصلی هست که برای تولید کد برنامه استفاده می کنیم همه
 را قرار می دهد .\n\n همه قسمت های برنامه را چک می کند و بین انها 
در قسمت تعریف تابع نام، نوع بازگشتی، پارامترها و بدنه تابع را به صورت متنی می‌سازیم .
در قسمت بعد بدنه ی {} را بررسی می کنیم به جهت اینکه کد های مرده ای که به صورت رشته به بلوک اضافه شده را هم پشتیبانی کنیم .
در مرحله بعد باید برای شرط های موجود ، کد تولید کنیم . 
 را تولید می کند .Returnو  Whileو for  ،If - else به ترتیب دستورات 
 عبارت های ریاضی را تولید visitAdditiveExpressionسپس در قسمت 
می کند . 
 به شکل متنی کد تابع AST در قسمت آخر با استفاده از فراخوانی تابع از 
 می رسیم .
 در واقعابتدا فایل کد مینی سی مون را می خوانیم و بعد Main در کد 
 را روی آن اجرا می کنیم .و در Obfuscation عملیات های خواسته شده در 
آخر کد مبهم شده را تحویل میگیریم .
Parser  که ورودی را به توکن های زبانی تبدیل می کند را ایجاد می کنیم .Lexer در ابتدا 
از توکن ها استفاده می کند تا یک درخت نحوی از کد ما برایمان بسازد .سپس یک نمونه از کلاس تغیر دهنده ی کد می سازیم و عملیات های بازنویسی را روی آن انجام میدهیم .
سپس یک درخت نحوی از کد جدید تولید شده می سازیم . 
code = code_generator.visit(obfuscated_ast)
در این قسمت درخت ساخته شده از کد مبهم شده مون را به یک کد متنی تبدیل 
Output.mc  را می خواند و کد تغیر یافته راinput.mc می کنیم . در آخر فایل
می نویسد .

