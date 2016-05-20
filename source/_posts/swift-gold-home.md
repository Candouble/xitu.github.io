---
title: 稀土每周技术分享-Swift/可视化编程
date: 2016-04-21 10:39:40
tags:
- Swift
- iOS
---

 
![效果图](https://dn-coding-net-production-pp.qbox.me/b2293876-f463-4211-8c00-796c4782a24b.png) 


## 前言
本次的主题是掘金每周三技术分享,这次轮到我们iOS了
现在Swift比较火再加上想向其他端的朋友炫耀一下我们iOS的可视化编程所以这次选择的主题就是:
Swift语言+可视化编写掘金主页

<!-- more -->

## 正文
本次分享由两部分组成
第一部分是用`Playground`来做Swift语法讲解,`Playground`真的很适合学习语法来使用,在真实开发环境中的作用还比较小...
第二部分是用一个真实项目来讲swift语言在真实开发环境中的作用和可视化编程的演示,亮点是用cell的xib自己计算高度

#### swfit基础语法

我对其他语言还不太熟悉,所以我就从OC角度来分析swift
>常量和变量

```
let a = 10;// 常量
var b = 10;// 变量
```

>可变数组和不可变数组,对应OC的NSArray和NSMutableArray

```
// 不可变数组
let arrA = [a,b]

// 可变数组
var arrB = [a]
arrB.append(b)
```

>下面是一些数组的黑科技

```
// 声明一个数组
let arrC = [1,2,3,4,5,6,7,8,9,10]

// 返回一个数组,值为每个元素是之前的两倍
arrC.map { (i) -> Int in
    return i*2
}

// 返回数组总和
arrC.reduce(0,combine: +)
```

>字典

```
// 不可变字典
let dicA = ["a":a,"b":b]

// 可变字典
var dicB = ["a":a]
// update类似于以前OC的SetValue ForKey 这里会自动检测是否存在这个key如果存在就是更新操作,如果不存在就是保存操作
dicB.updateValue(b, forKey: "b")
```

>元祖 这个应该是Swift独有了,可以把它理解为一个不受类型限制的字典,他的两个值可以使任意类型

```
// 元组
let group:(Int,Array) = (1,arrA)

// 利用元组交换两个数的值
var c = 1, d = 2
(c,d) = (d,c)
```

>block

```
// block
// block
let block = {(a:String) in print(a)
}
block("这是一个block")

// 带block的函数
typealias myBlock =  (b:String) ->Void
func blockFunction(a:String,block:myBlock){
    block(b: a)
}
blockFunction("这是一个带block的函数") { (aaaaa) in
    print(aaaaa)
}
```

>分支和枚举

```
// 分支和枚举
// if else
if a == 10 {

}else{

}

// while
while a != 10 {
    
}

// 普通的枚举
enum studyEnum:Int {
    case enumA = 1,enumB,enumC
}

// 枚举支持任意类型
enum specialEnum:String {
    case 吃了吗 = "吃了吗"
    case 没吃呢 = "没吃呢"
    case 该吃💊了 = "该吃💊了"
}
// 通过字符串创建枚举,创建后自带optional
let specialEnumAAA = specialEnum(rawValue:"该吃💊了")

// switch
// 可选值 需要拆包 optional后面讲解
switch specialEnumAAA! {
    case .吃了吗:

        break
    case .没吃呢:
    
        break
    case .该吃💊了:
        print(specialEnumAAA!.rawValue)
        break
}

```

>函数和类

```
// 类
class ClassA: NSObject {
    let a:String!
    override init() {
        a = "10"
    }
}
let classA = ClassA()

// 函数
func thisIsAFuncthion(a:String){
    print(a)
}
thisIsAFuncthion("这是一个函数")


// 函数式
// Swift中函数是一个一等值
// 代表着在swift中可以把一个函数当做参数来使用


func functionPrograming(strA:String) -> (str:String) -> String {
    
    func sayHello(strB:String) -> String {
        return strB + "," + strA
    }
    return sayHello
}

let funcA = functionPrograming("World")
funcA(str: "你好")
funcA(str: "Hello")
functionPrograming("World")(str:"你好")

// 上边那个函数可以这样写,结合block可能好理解一些
typealias StrAddStr = (str:String) -> String
func functionProgramingB(strA:String) -> StrAddStr {
    // strB:第二个函数传进来的参数
    return { strB in
        return strB + "," + strA
    }
}

```

>可选值,这是swift的新特性,每个可能为空的值都在外名包一层optional,用来保证安全

```
// 可选值
let str:String?
let number:Int?
let float:Float?


str = "这是一个可选值"
print(str)

// 两种拆包方法
// if let 解包,拆包出来的值作用于在if里
if let a = str {
    print(a)
}


//guard 解包,拆除来的值在之后都可以用,else{}里只执行没有值得方法
// return在playground里面无法使用,之后会在项目中演示
//guard let StrA = str else{
// return
//}
```
>playground中演示一下创建控件

```
// UIKit框架
let view = UIView(frame: CGRectMake(0,0,200,100))
view.backgroundColor = UIColor.lightGrayColor()
let label = UILabel(frame:  CGRectMake(0,0,100,50))
label.center = view.center
label.font = UIFont.systemFontOfSize(15)
label.text = "这是一个标题"
view.addSubview(label)
```
>效果图

![图片](https://dn-coding-net-production-pp.qbox.me/aa09f712-7655-4eec-94cd-0b02af84d857.png) 



#### 可视化编程
可视化这部分主要讲的就是用Xib和StoaryBoard来创建控件并和代码结合使用
亮点是用Xib创建的Cell自己来计算cell的高度


>先创建一个工程,选择Single View Application,选择swfit语言
 

![图片](https://dn-coding-net-production-pp.qbox.me/5fe33dfd-0144-4dff-93d8-a48f7bfcd853.png) 


>然后创建podfile,它类似于js的nam和其他语言的包管理,然后在命令行里选择工程路径 输入pod install 指令导入包

![图片](https://dn-coding-net-production-pp.qbox.me/8cd2050e-b249-498f-a4b8-d53dc16a3604.png) 


>然后打开workSpace选择Main.storyboard删除原来的控制器创建一个Table View Controller

![图片](https://dn-coding-net-production-pp.qbox.me/2032b871-3a48-4b69-a5c6-3e7cfc02baf7.png) 

>,然后在工程里创建一个继承自UITableViewController的类与之关联

![图片](https://dn-coding-net-production-pp.qbox.me/995ea9a0-aee6-4c14-b1d2-03c864333d54.png) 

![图片](https://dn-coding-net-production-pp.qbox.me/ca1b2026-6749-4dfd-a08e-aa1ed5adb528.png) 

![图片](https://dn-coding-net-production-pp.qbox.me/376a52cf-29d9-49a8-a126-be53f1de0bb5.png) 


 

>讲解一下tableView里面的代码,里面所牵扯的一些其他代码可以去相对应的类中去看

```
import UIKit


class XTHomeTableViewController: UITableViewController {

    var dataArr:Array<EntryModel> = []
    
    // 页面加载时会调用的方法
    override func viewDidLoad() {
        // 执行一下父类的方法
        super.viewDidLoad()
        
        // 从网络获取数据
        Helper.sharedInstance.fetchHotEntry(0) { (buyers) in
            // 将自己持有的数组等于网络请求的数组
            self.dataArr = buyers
            // 获取玩数据刷新页面
            self.tableView.reloadData()
        }
        
        // 让tableView的Cell高度按照可视化的约束来赋值
        self.tableView.rowHeight = UITableViewAutomaticDimension;
        // 注册cell
        self.tableView.registerNib(UINib.init(nibName: "XTHomeCell", bundle: nil), forCellReuseIdentifier: "XTHomeCell")
    }



    // MARK: - Table view data source
    // 区数
    override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        // #warning Incomplete implementation, return the number of sections
        return dataArr.count
    }

    // 行数
    override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // #warning Incomplete implementation, return the number of rows
        return 1
    }
    
    // 预估cell高度,方便xib自己计算高度 并提高性能
    override func tableView(tableView: UITableView, estimatedHeightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat {
        // 预估cell高度
        return 246
    }
    // 区尾高度,cell间留白效果
    override func tableView(tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat {
        return 10
    }
    
    // 返回cell
    override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCellWithIdentifier("XTHomeCell", forIndexPath: indexPath) as! XTHomeCell
        cell.setEntryModel(dataArr[indexPath.section])
        return cell
    }
    
    // 点击事件
    override func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
       
        // 根据给的identifier,来从stoaryBoard中找到需要跳转到的控制器
        let url = dataArr[indexPath.section].urlStr
        self.performSegueWithIdentifier(segue_webVC, sender:url)
    }
    // 根据给的identifier,来从stoaryBoard中找到需要跳转到的控制器
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        // 取出下一个控制器并赋值
        if segue.identifier == segue_webVC {
            let webVC = segue.destinationViewController as! XTEntryDetialViewController
            // 一个可选值,可能为空
            webVC.str = sender as? String
        }
    }
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

}
```

>cell,这里会用几个gif图来详细演示一下cell通过可视化自己计算高度的操作

>拖入控件

![图片](https://dn-coding-net-production-pp.qbox.me/e098746f-7eab-40d3-8b4f-784b9dce4776.gif) 

>普通约束
![图片](https://dn-coding-net-production-pp.qbox.me/489ff7fc-d2fa-43f0-9ff2-f70393f1cfce.gif) 

>计算高度所需要约束

![图片](https://dn-coding-net-production-pp.qbox.me/66b84dda-1832-4e4e-b30e-e4b0f307b7c6.gif) 


>然后是控件连线

![](https://dn-coding-net-production-pp.qbox.me/8934d764-55b2-4322-977d-a9a7664d727a.gif) 

>然后只需要在cell里面写赋值方法就可以了

```
      func setEntryModel(entry:EntryModel){
        // sd方法是管理图片下载的第三方,会自动缓存和取缓存,还做了一些线程优化
        entryimageView.sd_setImageWithURL(entry.screenshot)
        avatarImage.sd_setImageWithURL(entry.userAvatar)
        titleLabel.text = entry.title
        detailLabel.text = entry.content
    }
```



```
// 约束完了tableView里面写预估高度和这个方法,就可以让cell自己计算自己的高度了
self.tableView.rowHeight = UITableViewAutomaticDimension;
```
>其他代码,Model

```
class EntryModel: AVObject {
    var title:String? // 标题
    var content:String? // 描述
    var urlStr:String? // 网页url
    var userAvatar:NSURL? // 头像
    var screenshot:NSURL? // 顶部图片
}

```
>其他代码,网络层

```
typealias buyersBlock = (buyers:Array<EntryModel>) -> Void
class Helper: NSObject {
    static let sharedInstance = Helper()
    private override init() {}
    // leanCloud网络请求
    func fetchHotEntry(page:Int,callBack:buyersBlock){
        let query = AVQuery(className: entry_className)
        query.limit = 20
        query.skip = 20*page
        query .orderByDescending("hotIndex")
        query.includeKey("user")
        // 缓存
        query.cachePolicy = .CacheElseNetwork;
        query.findObjectsInBackgroundWithBlock { (objs, error) in
            if error != nil {
                return
            }
            callBack(buyers: self.resolveEntryWithObjs(objs))
        }
    }
    // 取一些自己想用的值
    func resolveEntryWithObjs(objs:[AnyObject!]) -> Array<EntryModel>{
        var arr:Array<EntryModel> = []
        for obj in objs{
            let dic = (obj as! AVObject).dictionaryForObject()
            let entry = EntryModel(className: entry_className)
            entry.content = dic.valueForKey("content") as? String
            entry.title = dic.valueForKey("title") as? String
            entry.urlStr = dic.valueForKey("url") as? String
            let imageUrlStr = dic.valueForKey("screenshot")?.valueForKey("url") as? String
            entry.screenshot = NSURL(string: imageUrlStr!)
            let userAvatar = dic.valueForKey("user")?.valueForKey("avatar_large") as? String
            if let urlStr =  userAvatar{
                entry.userAvatar =  NSURL(string: urlStr)
            }
            arr.append(entry)
        }
        return arr
    }
}
```
# 效果图
 


![效果图](https://dn-coding-net-production-pp.qbox.me/1fdee149-f72c-4336-a5d7-2de2550e4f51.gif) 

### 代码:https://github.com/Lafree317/SwiftGoldHome
可视化这边不太好用文字描述,如果将的不清楚可以私信我,或者去demo里面自己研究一下,希望各个水平的同学都可以从本次分享了解一些自己想要了解的东西...
本人是掘金iOS打杂的..水平菜,轻喷...