# objc-0825
用法总结：NSNumber、NSString、NSDate、NSCalendarDate、NSData
NSNumber
+ (NSNumber *)numberWithInt:(int)value;
+ (NSNumber *)numberWithDouble:(double)value;
- (int)intValue;
- (double)doubleValue;

NSNumber可以将基本数据类型包装起来，形成一个对象，这样就可以给其发送消息，装入NSArray中等等。
NSNumber * intNumber=[NSNumber numberWithInt：100]；
NSNumber *floatNumber=[NSNUmber numberWithFloat:100.00];
int i=[intNumber intValue]；
if([intNumber isEqualToNumber:floatNumber]) ....
NSNumber继承NSObject ，可以使用比较 compare： isEqual等消息

NSString
一个NSString对象可以存储一段Unicode字符。在cocoa中，所有和字符、字符串相关的处理都是使用NSString来完成。
NSObject -> NSString     // NSString继承自NSObject
+(id) stringWithContentsOfFile:path encoding:enc error:err;
+(id) stringWithContentsOfURL:url encoding:enc error:err;
+(id) stringWithString:nsstring;   //创建一个新的字符串，并将其设置为nsstring
-(id)initWithFormat:(NSString *)format, ...  ;
-(id)initWithString:nsstring;     //将分配的字符串设置为nsstring
- (BOOL)isEqualToString:(NSString *)string;
- (BOOL)hasPrefix:(NSString *)string;
- (int)intValue;
- (double)doubleValue;

- (NSString *)stringByAppendingString:(NSString *)string;  // 给一个字符串附加一个字符串string。
- (NSString *)stringByAppendingFormat:(NSString *)string;
- (NSString *)stringByDeletingPathComponent;
 
-----创建字符串的方法-----
//1、创建常量字符串
    NSString *astring = @"This is a String!";  
//2、先创建一个空的字符串，然后赋值；
//    alloc和init组合则适合在函数之间传递参数，用完之后需要手工release
    NSString *astring = [[NSString alloc] init];
    astring = @"This is a String!";
    NSLog(@"astring:%@",astring);
    [astring release];
//3、在以上方法中，提升速度:initWithString方法
    NSString *astring = [[NSString alloc] initWithString:@"This is a String!"];
    NSLog(@"astring:%@",astring);
    [astring release];
//4、创建临时字符串
    NSString *astring;
    astring = [NSString stringWithCString:"This is a temporary string"];
    NSLog(@"astring:%@",astring);
// OR
    NSString *  scriptString = [NSString stringWithString:@" tell application \"Mail\"\r"];
//5、创建格式化字符串:占位符（由一个%加一个字符组成）
    int i = 1;
    int j = 2;
    NSString *astring = [[NSString alloc] initWithString:[NSString stringWithFormat:@"%d.This is %i string!",i,j]];
    NSLog(@"astring:%@",astring);
    [astring release];
-----从文件读取字符串-----
    NSString *path = @"astring.text";
    NSString *astring = [[NSString alloc] initWithContentsOfFile:path];
    NSLog(@"astring:%@",astring);
    [astring release];

-----写字符串到文件----    
    NSString *astring = [[NSString alloc] initWithString:@"This is a String!"];
    NSLog(@"astring:%@",astring);
    NSString *path = @"astring.text";   
    [astring writeToFile: path atomically: YES];
    [astring release];   
-----比较两个字符串-----
//1、用C比较:strcmp函数
    char string1[] = "string!";
    char string2[] = "string!";
    if(strcmp(string1, string2) = = 0)
    {
        NSLog(@"1");
    }
 //2、isEqualToString方法   
    NSString *astring01 = @"This is a String!";
    NSString *astring02 = @"This is a String!";
    BOOL result = [astring01 isEqualToString:astring02];
    NSLog(@"result:%d",result);
//3、compare方法(comparer返回的三种值：NSOrderedSame，NSOrderedAscending，NSOrderedDescending)   
    NSString *astring01 = @"This is a String!";
    NSString *astring02 = @"This is a String!";   
    BOOL result = [astring01 compare:astring02] = = NSOrderedSame;   //NSOrderedSame 判断两者内容是否相同
    NSLog(@"result:%d",result);   

    NSString *astring01 = @"This is a String!";
    NSString *astring02 = @"this is a String!";
    BOOL result = [astring01 compare:astring02] = = NSOrderedAscending;   
    NSLog(@"result:%d",result);
    //NSOrderedAscending 判断两对象值的大小(按字母顺序进行比较，astring02大于astring01为真)

    NSString *astring01 = @"this is a String!";
    NSString *astring02 = @"This is a String!";
    BOOL result = [astring01 compare:astring02] = = NSOrderedDescending;   
    NSLog(@"result:%d",result);     
    //NSOrderedDescending 判断两对象值的大小(按字母顺序进行比较，astring02小于astring01为真)
 //4、不考虑大小写比较字符串1
    NSString *astring01 = @"this is a String!";
    NSString *astring02 = @"This is a String!";
    BOOL result = [astring01 caseInsensitiveCompare:astring02] = = NSOrderedSame;   
    NSLog(@"result:%d",result);     
    //NSOrderedDescending判断两对象值的大小(按字母顺序进行比较，astring02小于astring01为真)
//5、不考虑大小写比较字符串2
    NSString *astring01 = @"this is a String!";
    NSString *astring02 = @"This is a String!";
    BOOL result = [astring01 compare:astring02
                            options:NSCaseInsensitiveSearch | NSNumericSearch] = = NSOrderedSame;   
    NSLog(@"result:%d",result);     
    //NSCaseInsensitiveSearch:不区分大小写比较 NSLiteralSearch:进行完全比较，区分大小写 NSNumericSearch:比较字符串的字符个数，而不是字符值。

-----改变字符串的大小写-----
    NSString *string1 = @"A String";
    NSString *string2 = @"String";
    NSLog(@"string1:%@",[string1 uppercaseString]);//uppercaseString返回转换为大写的字符串
    NSLog(@"string2:%@",[string2 lowercaseString]);//lowercaseString返回转换为小写的字符串
    NSLog(@"string2:%@",[string2 capitalizedString]);//capitalizedString返回每个单词首字母大写的字符串

-----在串中搜索子串 -----        

    NSString *string1 = @"This is a string";
    NSString *string2 = @"string";
    NSRange range = [string1 rangeOfString:string2];
    int location = range.location;
    int leight = range.length;
    NSString *astring = [[NSString alloc] initWithString:[NSString stringWithFormat:@"Location:%i,Leight:%i",location,leight]];
    NSLog(@"astring:%@",astring);
    [astring release];

-----抽取子串 -----     

//1、-substringToIndex: 从字符串的开头一直截取到指定的位置，但不包括该位置的字符
    NSString *string1 = @"This is a string";
    NSString *string2 = [string1 substringToIndex:3]; 
    NSLog(@"string2:%@",string2);

//2、-substringFromIndex: 以指定位置开始（包括指定位置的字符），并包括之后的全部字符
    NSString *string1 = @"This is a string";
    NSString *string2 = [string1 substringFromIndex:3];
    NSLog(@"string2:%@",string2);

//3、-substringWithRange: //按照所给出的位置，长度，任意地从字符串中截取子串
    NSString *string1 = @"This is a string";
    NSString *string2 = [string1 substringWithRange:NSMakeRange(0, 4)];
    NSLog(@"string2:%@",string2);

//4、快速枚举
    for(NSString *filename in direnum)    {
        if([[filename pathExtension] isEqualToString:@"jpg"]){
            [files addObject:filename];
        }
    }
    NSLog(@"files:%@",files);
//5、枚举
    NSEnumerator *filenum;
    filenum = [files objectEnumerator];
    while (filename = [filenum nextObject]) {
        NSLog(@"filename:%@",filename);
    }

@"b",@"a",@"e",@"d",@"c",@"f",@"h",@"g",nil];   
    NSLog(@"oldArray:%@",oldArray);
    NSEnumerator *enumerator;
    enumerator = [oldArray objectEnumerator];
    id obj;
    while(obj = [enumerator nextObject])    {
        [newArray addObject: obj];
    }
    [newArray sortUsingSelector:@selector(compare:)];
    NSLog(@"newArray:%@", newArray);
    [newArray release];
-----切分数组-----
//1、从字符串分割到数组－ componentsSeparatedByString:
    NSString *string = [[NSString alloc] initWithString:@"One,Two,Three,Four"];
    NSLog(@"string:%@",string);   
    NSArray *array = [string componentsSeparatedByString:@","];
    NSLog(@"array:%@",array);
    [string release];

//2、从数组合并元素到字符串- componentsJoinedByString:
    NSArray *array = [[NSArray alloc] initWithObjects:@"One",@"Two",@"Three",@"Four",nil];
    NSString *string = [array componentsJoinedByString:@","];
    NSLog(@"string:%@",string);

-----从目录搜索扩展名为jpg的文件-----
//NSFileManager *fileManager = [NSFileManager defaultManager];
    NSString *home;
    home = @"../Users/";
    NSDirectoryEnumerator *direnum;
    direnum = [fileManager enumeratorAtPath: home];
    NSMutableArray *files = [[NSMutableArray alloc] init];
//枚举
    NSString *filename;
    while (filename = [direnum nextObject]) {
        if([[filename pathExtension] hasSuffix:@"jpg"]){
            [files addObject:filename];
        }
    }
//扩展路径 
    NSString *Path = @"~/NSData.txt";
    NSString *absolutePath = [Path stringByExpandingTildeInPath];
    NSLog(@"absolutePath:%@",absolutePath);
    NSLog(@"Path:%@",[absolutePath stringByAbbreviatingWithTildeInPath]);
//文件扩展名
    NSString *Path = @"~/NSData.txt";
    NSLog(@"Extension:%@",[Path pathExtension]);

-----查找与替换-----

- (NSString *)stringByReplacingCharactersInRange:(NSRange)range withString:(NSString *)replacement

- (NSString *)stringByReplacingOccurrencesOfString:(NSString *)target withString:(NSString *)replacement

NSMutableString(可修改的字符串)
NSObject -> NSString -> NSMutableString 
Common NSMutableString methods
+ (id)string;
- (void)appendString:(NSString *)string;
- (void)appendFormat:(NSString *)format, ...;
-----给字符串分配容量-----
    //stringWithCapacity:
    NSMutableString *String;
    String = [NSMutableString stringWithCapacity:40];
-----在已有字符串后面添加字符-----   

    //appendString: and appendFormat:
    NSMutableString *String1 = [[NSMutableString alloc] initWithString:@"This is a NSMutableString"];
    //[String1 appendString:@", I will be adding some character"];
    [String1 appendFormat:[NSString stringWithFormat:@", I will be adding some character"]];
    NSLog(@"String1:%@",String1);
----- 在已有字符串中按照所给出范围删除字符----   
     //deleteCharactersInRange:
     NSMutableString *String1 = [[NSMutableString alloc] initWithString:@"This is a NSMutableString"];
     [String1 deleteCharactersInRange:NSMakeRange(0, 5)];    // 删除指定范围（location=0，length=5）的字符串
     NSLog(@"String1:%@",String1);
----在已有字符串后面在所指定的位置中插入给出的字符串-----
    //-insertString: atIndex:
    NSMutableString *String1 = [[NSMutableString alloc] initWithString:@"This is a NSMutableString"];
    [String1 insertString:@"Hi! " atIndex:0];
    NSLog(@"String1:%@",String1);
    [String1 insertString:@"and StringEnd", atIndex:[String1 length]];  //  在可变字符串的最后插入
----将已有的空符串换成其它的字符串-----
    //-setString:
    NSMutableString *String1 = [[NSMutableString alloc] initWithString:@"This is a NSMutableString"];
    [String1 setString:@"Hello Word!"];
    NSLog(@"String1:%@",String1);

----查找-----
   NSRange subRange = [String1 rangeOfString:@"is a"];   // 如果没查找到，则 （subRange.location == NSNotFound）为真。

----按照所给出的范围替换的原有的字符-----
    //-setString:
    NSMutableString *String1 = [[NSMutableString alloc] initWithString:@"This is a NSMutableString"];
    [String1 replaceCharactersInRange:NSMakeRange(0, 4) withString:@"That"];     // 用于NSMutableString
    NSLog(@"String1:%@",String1);

----在给定的范围内查找并替换-----
- (NSUInteger)replaceOccurrencesOfString:(NSString *)target withString:(NSString *)replacement options:(NSStringCompareOptions)opts range:(NSRange)searchRange
----判断字符串内是否还包含别的字符串(前缀，后缀)-----
//01： 检查字符串是否以另一个字符串开头- (BOOL) hasPrefix: (NSString *) aString;
    NSString *String1 = @"NSStringInformation.txt";
    [String1 hasPrefix:@"NSString"] = = 1 ?  NSLog(@"YES") : NSLog(@"NO");
    [String1 hasSuffix:@".txt"] = = 1 ?  NSLog(@"YES") : NSLog(@"NO");

//02： 查找字符串某处是否包含给定的字符串 - (NSRange) rangeOfString: (NSString *) aString，这一点前面在串中搜索子串用到过

NSRange subRange;
subRange = [string1 rangeOfString:@"string A"];  //查找字符串string1中是否包含“string A”。返回NSRange类型。
if(subRange.location == NSNotFound)
     NSLog(@"String not found ");
else  NSLog(@"string is at index %lu, length is %lu", subRange.location, subRange.length);

NSDate



NSCalendarDate

NSCalendarDate对象包含了日期和时间、时区以及一个带有格式的字符串，它从NSDate继承而来。

NSCalendarDate对象是immutable的，一旦被创建，无法修改其中的时间和日期，当然可以修改那个带格式的字符串和时区。

以下是常用方法：

+(id)calendarDate;  //创建当前日期和时间以及默认格式的NSCalendarDate对象，时区为机器设置好的时区。


+(id)dateWithYear:(int)year
    month:(unsigned)month
      day:(unsigned)day 
     hour:(unsigned)hour
   minute:(unsigned)minute
   second:(unsigned)second 
 timeZone:(NSTimeZone  *)aTimeZone 
-(int)dayOfCommonEra;  //得到从公元1年算起，有多少天

-(int)dayOfMonth;          //返回是月的第几天（1-31）

-(int)dayOfWeek;          //返回是周的第几天 （0-6）

-(int)dayOfYear;          //返回是年的第几天（1-366）

-(int)hourOfDay;          // 返回是日的第几个小时（0-23）

-(void)setCalendarFormate:(NSString *)format 

--------创建NSCalendarDate对象--------

NSCalendarDate *now;

now = [NSCalendarDate calendarDate];


NSTimeZone *pacific = [NSTimeZone timeZoneWithName:@"PST"];
NSCalendarDate *hotTime = [NSCalendarDate dateWithYear:2011 month:2 day:3 hour:14 minute:0 second:0 timeZone:pacific];
NSData

使用文件时，需要频繁地将数据读入一个临时存储区，它通常成为缓冲区。

NSData类提供了一种简单的方式，它用来设置缓冲区、将文件的内容读入缓冲区，或将缓冲区的内容写到一个文件。

对于32位应用程序，NSDATA缓存区最多可以存储2GB的数据。

我们既可定义不变缓冲区（NSData类），也可定义可变的缓冲区（NSMutableData类）。

下面代码展示了如何将文件的内容读入内存缓冲区，然后再将缓冲区的内容写入到另一个文件中。

NSData *fileData;
NSFileManager *fileManager = [[NSFileManager alloc]init];
fileData = [fileManager contentsAtPath:path];  
[fileManager createFileAtPath:path2 contents:fileData attributes:nil];   //采用默认的属性值

类型转换 NSData -> NSString：

NSString *strData = [[NSString alloc]initWithData:fileData encoding:NSASCIIStringEncoding];

类型转换 NSString -> NSData：

NSData *fileData2 = [strData dataUsingEncoding:NSUTF8StringEncoding];

NSMutableData
