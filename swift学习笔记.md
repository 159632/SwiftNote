#  swift学习笔记

1. 在当前类中访问属性或者方法的时候 可以省略 self. 推荐不写, 后面的闭包中必须添加self.

2. 构造函数 [xxx alloc] initWithXXX] ==> xxx(XXX:) ;    [alloc init] => ()

3. Swift没有宏这个概念.     deinit 类似于 dealloc

4. Swift没有类扩展这个概念

5. 不需要导入头文件,可以直接引用其他类里面的方法或属性

6. Swift非常注重格式,比如 a = 3 ,在=的左右必须都有一个空格,或同时有多个,当=左右空格个数不一样时,比如 b= 2 这样会报错

7. typealias Void = ( )  // 空元组 

8. 数组

   ```swift
   // Array<Element>  等价于  [Element]
   // 集合（Set）用来存储相同类型并且没有确定顺序的值
   // 字典使用 Dictionary<Key, Value> 等价于 [Key: Value] 
   // Swift 中的 Set 类型被写为 Set<Element> 没有等价模式
   var letters = Set<Character>()
   var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
   
   let arr : [Any] = ["阿达"，18]   //不推荐在数组中放不同的元素
   var empatyArray = [String]()  //var表示可变数组,常量类型的空数组没有意义
   // 同时便利角标和对应值
   // 如果我们同时需要每个数据项的值和索引值，可以使用 enumerated() 方法来进行数组遍历。enumerated() 返回一个由每一个数据项索引值和数据值组成的元组
   for (index ,value) in arr.enumerated(){
      print(index)
      pring(value)
   }
   //数组的合并
   let arr1: [Any] = ["老李"，19]
   let arr2: [Any] = ["老张"，22]
   let arr3 = arr1 + arr2
   print(arr3)
   ```

9. 字典

   ```swift
   let dict: [String : Any] = ["name" : "波仔" , "age" : 21] // 字典同样是使用[]来声明，[String : Any] 是最常见的字典格式
   var emptyDict = [String : Any]() // 声明空字典
   var dic: [String : Any] = ["name" : "波仔" , "age" : 33]
   //增加
   dic['love'] = '菠萝'
   // 更改
   dic['love'] = '梨'
   // 删除
   dic.removeValue(forKey: "age")
   //遍历
   for (key,value) in dic {
     print(key,value)
   }
   ```

10. 函数

    ```swift
    //可 变参数的传入值在函数体中变为此类型的一个数组。例如，一个叫做 numbers 的 Double... 型可变参数，在函数体内可以当做一个叫 numbers 的 [Double] 型的数组常量。
    func arithmeticMean(_ numbers: Double...) -> Double {
        var total: Double = 0
        for number in numbers {
            total += number
        }
        return total / Double(numbers.count)
    }
    
    //使用 # 能简化外部参数名的定义，num1、num2既是形式参数名 又是外部参数名
    func sum(#num1 : Int, #num2 : Int)
    {
       return num1 + num2
    }
    //调用函数
    sum(num1: 10, num2: 20)
    ```

    ```swift
    // 带有默认参数值的形参，swift会自动给它生成一个跟形参名称相同的外部参数名
    // 参数 age: Int = 20 相当于 #age: Int = 20
    // 在带有默认参数值的参数名前加个 下划线_，调用函数是就不用写外部参数名
    // 传入的参数默认为let 内部不可修改参数。 若想内部修改参数 使用var修饰
    func  addStudent(name: String, _ age : Int = 20){
      print("添加一个学生：name=\(name), age=\(age)")
    }
    
    addStudent("Jack", 25)
    ```

    ```swift
    //在c语言中，利用指针可以在函数内部修改外部变量的值
    //在swift中，利用输入输出参数，也可以在函数内部修改外部变量的值
    //输入输出参数，即 能输入一个值进来，也可以输出一个值到外部
    // 在参数名前加个inout关键字即可
    // 输入输出参数不能再使用let 或者var. (inout 和 var let 不能共存)
    func swap(inout a : Int , inout b : Int){
        a = a + b
        b = a - b
        a = a - b
    }
    var c = 10
    var d = 20
    swap(&c,&d) // 传入的参数 必须是变量必须加上& , 不能传入常量 或者 字面量
    
    // 方法二。使用多元组
    func swap<T>(_ a: inout T, _ b: inout T) {
        (a, b) = (b, a)
    }
    ```

11. as  as!  as! 的三种操作符的区别    

    ```swift
    //1.从派生类 向上转型 为基类
    calss Person { // 定义基类
      var name : String
      init (_ name : String){
        self.name = name
      }
    }
    class Student : Person{ // 学生类
    }
    clsss Teacher : Person{ // 教师类
    }
    func showPersonName (_ people : Person){ // 参数为基类对象的方法
      let name = people.name
      print("\(name)")
    }
    
    var tom = Student("Tom")
    var teacher = Teacher("Kevin")
    
    let person1 = tom as Person // 把子类对象 向上转型 为一般的人员对象
    showPersonName（person1） // 再调用处理人员对象的函数
    
    //2.消除二义性
    let age = 28 as Int
    
    //3. switch语句进行模式匹配
    switch person1 {
      case let person1 as Student:
          print("是学生类型")
      case let person1 as Teacher:
          print("是教师类型")
      default:break
    }
    ```

    ```swift
    // as! 向下转型 时使用。由于是强制类型转换，如果转换失败会报 runtime运行错误
    let person : Person = Teacher("Kevin")
    let kevin = person as! Teacher
    ```

    ```swift
    // as? 和 as! 转换规则一样。 但是as? 若是转换不成功 便会返回一个nil对象。由于as?在转换失败的时候也不会出现错误，所以只有在100%保证转换成功的时候才会使用as!
    let person :Person = Teacher("Kevin")
    if let someone = person as? Teacher{
      print("老师姓名\(someone.name)")
    }else{
       print("这个人不是老师")
    }
    ```

12. 一些常用的第三方库

    ```swift
    pod 'RxSwift'
    pod 'RxCocoa'         # 把UI库和RxSwift结合
    pod 'DZNEmptyDataSet' #
    pod 'HandyJSON'       #
    pod 'ObjectMapper'    # Json转模型
    pod 'SnapKit'         # 跟Masonry一样是用来设置约束的，swift版
    pod 'SwiftyJSON'      # Json数据转换
    pod 'Alamofire'       # 用于网络请求
    pod 'Moya/RxSwift'    # 用于网络请求
    pod 'Kingfisher'      # SDWebImage swift 版
    pod 'RxDataSources'   # RxSwift中用于设置UITableView/UICollectionView data sources
    
    ```
    
13. swift中 $0  $1 的含义

    ```swift
    //swift中用 $0 表示闭包中第一个参数，$1 表示闭包中第二个参数
    // 不使用$0 $1的闭包
    let numbers = [1,2,3,4,5,6]
    var sortNumbers = numbers.sorted(by: {(a , b) -> Bool in 
                                     return a < b
                                     })
    //使用$0,$1
    //使用$0、$1的话，参数类型可以自动判断，并且in关键字也可以省略，也就是只用写函数体就可以了
    let numbers = [1,2,3,4,5,6]
    var sortNumbers = numbers.sorted(by: {$0 < $1})
    print("numbers - " + "\(sortNumbers)")
    ```

14. [weak self] 与 [unowned self] 介绍

    ```swift
    // 用于解决闭包 循环引用引起的内存泄漏
    // 如果捕获（比如 self）可以被设置为 nil，也就是说它可能在闭包前被销毁，那么就要将捕获定义为 weak。
    // 如果它们一直是相互引用，即同时销毁的，那么就可以将捕获定义为 unowned。相当于unsafe-unretain 第二个界面有 闭包使用定时器，过快返回上级界面 可能导致闪退
    ```

15. 范型 <T>：元素可以是任何类型，但是所有元素又必须是同一种类型

    ```swift
    // 实现一个栈，栈里边的元素可以是任何类型，但是所有元素又必须是同一种类型，使用泛型实现的代码就是这样的
    calss YBStack<T> : NSObject{
      //栈空间
      private var list:[T] = []
      
      //进栈
      public func push(item:T){
        list.append(item)
      }
      //出栈
      public func pop() -> T {
        return list.removeLast()
      }
    }
    ```

16. Swift 5 中，新增了一个枚举类型 `Result`

    ```swift
    // `Result` 定义
    // 接收两个泛型参数，一个为Success，一个为Failure，但是Failure必须是Error类型的
    // Success, Failure 两种情况互斥
    public enum Result<Success, Failure> where Failure: Error{
      case success(Success) // Success代表正确执行的值, A success, storing a `Success` value.
      case failure(Failure) // Failure代表出现问题时的错误值, A failure, storing a `Failure` value.
    }
    ```

    ```swift
    // 1.定义Error
    enum NetworkError: Error {    
        case URLInvalid
    }
    
    // 定义一个函数，包含一个逃逸闭包进行异步回调
    func getInfo(from urlString: String, completionHandler: @escaping (Result<String, NetworkError>) -> Void)  {
    
        if urlString.hasPrefix("https://") {        
            // 经过一系列网络处理以后得到一个服务器返回的数据
            let data = "response result"        
            // 获取数据
            completionHandler(.success(data))
        }
    
        else{        
            // URL有问题
            completionHandler(.failure(.URLInvalid))
        }
    }
    
    // 调用函数
    getInfo(from: "https://www.baidu.com") { result in
    
        // 处理Result
        switch result {
    
        case .success(let content):        
            print(content)
    
        case .failure:
            // 如果参数不是https://开头 会打印
            print("url有问题")
        }
    }
    ```

    ```swift
    // 示例
    // ViewModel中声明 
    func openScene(sceneId: String, completion: @escaping (Result<Void,Error>) -> Void) {
            SmartSceneManager.openScenePower(sceneId: sceneId) { (response) in
                switch response {
                case .success(_):
                    completion(.success(()))
                case .failure(let error):
                    completion(.failure(error))
                }
            }
         }
    // VC中调用
    viewModel.openScene(sceneId: sceneId) {(response) in
                switch response {
                case .success(_):
                    nav.showMessage("已执行")
                case .failure(let error):
                    let message = (error as NSError).localizedDescription
                    nav.showMessage(message)
                }
      }
    
    ```

    

17. 协议、 类

18. protocol中的 associatedtype关键字: 用来定义一个在协议中的 “泛型” 类型 。定义为 associatedtype类型的 在实现协议的类中 使用的时候 指明该类型的 具体类型是什么就可以了

    ```swift
     protocol TestProtocol {
        associatedtype aaaaa
        func testFunc() -> aaaaa
    }
    
    class TestClassInt: TestProtocol {
        func testFunc() -> Int {
            return 1
        }
    
    }
    
    class TestClassDouble: TestProtocol {
    
         func testFunc() -> Double {
             return 1.666666666
         }
    }
    ```

    

19. 协议的拓展（Protocol Extension）

    ```swift
    // 可以给协议内部的函数添加实现、给协议添加新的函数。从而将函数功能添加到遵守协议的所有类型中。
    // 通过协议定义，提供实现的入口，遵循协议的类型需要对其进行实现
    // 协议扩展，为入口提供默认实现。根据入口提供额外实现
     @objc protocol Person {
        @objc optional var age: Int { get set}
        @objc optional func work()
    }
    //Person的扩展
     extension Person {
        func work(){
            print("我想上厕所")
        }
        func run() {
            print("我在跑步")
        }
    }
    
    //Man遵守protocol---- Person协议
    class Man:Person  {
        var age: Int = 20
        func run() {
            print("我是男的也在跑步")
        }
    }
    //Woman 遵守protocol---- Person 协议
    class Woman: Person {
        var age: Int = 18
        func work() {
            print("去女厕所上厕所")
        }
    }
    // 调用
    let man = Man()
    man.work()
    man.run()
    let woman = Woman()
    woman.work()
    woman.run()
    //面向协议编程(POP)的理解---->用定义协议来代替面向对象编程中的继承、多继承等，来达到使用灵活 和 解偶的目的
    //让Man类和Woman类遵守Person协议就可以达到面向对象编程中继承的效果
    //如果有多个协议要遵守的话就可以通过遵守多个协议来达到面向对象编程中的【多继承】的效果
    ```

20. 代理模式

    ```swift
    //在后面加上class关键字，一般加class ，
     protocol CCDelegate : class {
         func doSomething()
    }
    // 代理当作变量的时候用weak 修饰防止循环引用
    weak var  delegate: CCDelegate?
    ```

21. 闭包

    ```swift
    /*闭包的表达式：
    {（参数）-> 返回值 in
    //代码
    }*/
    //申明为变量：
    typealias Swiftblock = (String,Any) -> Void
    var testBlock: Swiftblock?
    //使用
    testBlock?("ddd","ffffff")
    //实现
    { [weak self] (a,b) in
        print(a)
        print(b)
    }
    //[weak self]是为了防止循环引用，(a,b)是传过来的参数
    ```

22. 可选协议

    ```swift
    //Swift protocol本身不允许可选项，要求所有方法都是必须得实现的。但是由于Swift和ObjC可以混编，那么为了方便和ObjC打交道，Swift支持在 protocol中使用 optional 关键字作为前缀来定义可选要求，且 协议和可选 要求都必须带上@objc属性。
    @objc protocol CounterDataSource {
        @objc optional func incrementForCount(count: Int) -> Int
        @objc optional var fixedIncrement: Int { get }
    }
    
    // 但是标记 @objc 特性的协议只能被继承自 ObjC 类的类或者 @objc 类遵循，其他类以及结构体和枚举均不能遵循这种协议。这对于Swift protocol是一个很大的限制。
    // 由于 protocol支持可扩展，那么我们可以在声明一个 protocol之后再用extension的方式给出部分方法默认的实现,这样这些方法在实际的类中就是可选实现的了。
    ```

23. Delegate

    ```swift
    // 错误写法
    // 编译器会提示错误'weak' must not be applied to non-class-bound 'MyProyocol'; consider adding a protocol conformance that has a class bound
    protocol MyProyocol {
        func method()
    }
    class MyClass: NSObject {
        weak var delegate: MyProyocol?
    }
    // 因为 Swift 的 protocol 是可以被 class 以外的其他类型遵守的，而对于像 struct 或是 enum 这样的类型，本身就不通过引用计数来管理内存，所以也不可能用 weak 这样的 ARC 的概念来进行修饰。
    // 因此想要在 Swift 中使用 weak delegate，我们就需要将 protocol 限制在 class 内，例如
    protocol MyProyocol: class {
        func method()
    }
    
    protocol MyProyocol: NSObjectProtocol {
        func method()
    }
    //class限制协议只用在class中，NSObjectProtocol限制只用在NSObject中，明显class的范围更广，日常开发中都可以使用。
    
    ```
    
24. ### @synchronized

    ```swift
    // 可以用来修饰一个变量，并为其自动加上和解除互斥锁，用以保证变量在作用范围内不会被其他线程改变。
    // Swift 中 @synchronized 已经不存在了。其实 @synchronized 在幕后做的事情是调用了 objc_sync 中的 objc_sync_enter 和 objc_sync_exit 方法，并且加入了一些异常判断。因此，在 Swift 中，如果我们忽略掉那些异常的话，我们想要 lock 一个变量的话，可以这样写：
    private var isResponse: Bool {
        get {
            objc_sync_enter(lockObj)
            let result = _isResponse
            objc_sync_exit(lockObj)
            return result
        }
        set {
            objc_sync_enter(lockObj)
            _isResponse = newValue
            objc_sync_exit(lockObj)
        }
    }
    ```

25. ### struct与class

    ```swift
    
    // 区别
    /*struct是值类型，class是引用类型。
      struct有一个自动生成的成员逐一构造器，用于初始化新结构体实例中成员的属性；而class没有。
      struct中修改 self 或其属性的方法必须将该实例方法标注为 mutating；而class并不需要。
      struct不可以继承，class可以继承。
      struct赋值是值拷贝，拷贝的是内容；class是引用拷贝，拷贝的是指针。
      struct是自动线程安全的；而class不是。
      struct存储在stack中，class存储在heap中，struct更快。*/
    
     //类更强大
    /*继承允许一个类继承另一个类的特征
      类型转换允许在运行时检查和解释一个类实例的类型
      析构器允许一个类实例释放任何其所被分配的资源
      引用计数允许对一个类的多次引用 */
    
    // 如果struct能够完全满足你的预期要求，可以多使用struct。因为 一般是使用最小的工具来完成你的目标
    
    //共同点
    /*定义属性用于存储值
      定义方法用于提供功能
      定义下标操作使得可以通过下标语法来访问实例所包含的值
      定义构造器用于生成初始化值
      通过扩展以增加默认实现的功能
      实现协议以提供某种标准功能*/
    ```

26. ### map与flatmap

    ```swift
    // 都会对数组中的每一个元素调用一次闭包函数，并返回该元素所映射的值，最终返回一个新数组。但flatmap更进一步，多做了一些事情：
    // 1.返回的结果中会去除nil，并且会解包Optional类型。
    // 2.会将N维数组变成1维数组返回。
    ```

27. defer

    ```swift
    //`defer` 所声明的 block 会在当前代码执行退出后被调用。正因为它提供了一种延时调用的方式，所以一般会被用来做资源释放或者销毁，这在某个函数有多个返回出口的时候特别有用。
    func testDefer() {
        print("开始持有资源")
        defer {
            print("结束持有资源")
        }
    
        print("程序运行ing")
    }
    
    // 开始持有资源
    // 程序运行ing
    // 结束持有资源
    ```

    使用`defer`会方便的将前后必要逻辑放在一起，增强可读性和维护，但是不正确的使用也会导致问题。例如上面的例子，在持有之前先判断是否资源已经被其他持有：

    ```swift
    func testDefer(isLock: Bool) {
        if !isLock {
            print("开始持有资源")
            defer {
                print("结束持有资源")
            }
        }
    
        print("程序运行ing")
    }
    
    // 开始持有资源
    // 结束持有资源
    // 程序运行ing
    
    ```

    我们要注意到`defer`的作用域不是整个函数，而是当前的`scope`。那如果有多份`defer`呢？

    ```swift
    func testDefer() {
        print("开始持有资源")
    
        defer {
            print("结束持有资源A")
        }
    
        defer {
            print("结束持有资源B")
        }
    
        print("程序运行ing")
    }
    
    // 开始持有资源
    // 程序运行ing
    // 结束持有资源B
    // 结束持有资源A
    ```

    当有多个`defer`时，后加入的先执行，可以猜测Swift使用了`stack`来管理`defer`。

28. 

29. 

30. static func 相当于class final func。禁止这个方法被重写

    clas func 可以被继承重写

31. 

32. 

33. 

34. 

35. 

36. 

37. 

38. 

39. 

40. 

41. 

42. 

43. 

44. 

    

    

    

    

    

    

    

    

