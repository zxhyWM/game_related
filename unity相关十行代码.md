# 一、十行代码学会Unity

## 0、前言

制作游戏是一个学习的过程

> Start()	游戏开始/第一帧时触发
>
> Update()	每帧触发一次
>
> OnTriggerEnter2D()	监测到碰撞时触发

## 1、变量 Variable

> 开放性
>
> public其他脚本也可以使用。需要频繁微调的数据也适合使用
>
> private局限在本脚本内部使用

> 数据类型
>
> | int   | float | bool        | string        | 引用变量              |
> | ----- | ----- | ----------- | ------------- | --------------------- |
> | 整数  | 小数  | 真假        | 字符串        | 其他脚本引用变量      |
> | 1、23 | 42.3  | True、False | “asd”、“自定” | GameObject、Transform |
>
> GameObject是游戏对象，在其之上添加脚本等组件

> 命名
>
> 小驼峰

## 2、组件引用 GetComponent<>()

> 创建一个引用变量，和组件绑定，这样在代码中调用组件时显得整洁、快速
>
> ```c#
> //组件名称SpriteRender,当前脚本所在的游戏对象gameObject
> private SpriteRender spriteRender;
> spriteRender =gameObject.GetComponent<SpriteRender>();
> 
> spriteRender.color=Color.black;
> ```

## 3、条件检测 if/else

就是条件判断，符合就触发定义的结果。条件判断多种多样，就用到了if/else。

> 条件触发
>
> ```
> if(你希望触发的条件)
> {
> 	你希望造成的结果;
> }
> ```

> 条件分支
>
> ```
> if(你希望触发的1号条件)
> {
> 	你希望造成的1号结果;
> }
> else if(你希望触发的2号条件)
> {
> 	你希望造成的2号结果;
> }
> ...
> else(你希望触发的n号条件)
> {
> 	你希望造成的n号结果;
> }
> ```

> 状态机，控制游戏对象状态变化，Animator
>
> 自定义动画种类，建立相互之间的联系。定义变量来作为状态改变的标准
>
> 点击连线，更改设置，达到合适的过渡效果。Has Exit Time是状态后摇，有些需要，有些不用。一般去掉勾选。
>
> 别忘了在条件中添加变量，改变时触发状态改变

> 实际操作，站立--跑步
>
> 如何识别移动？
>
> 每帧检测：记录玩家方向按键的变量*速度=>新坐标。
>
> 如果都是0，说明玩家没有按下方向键，那么不可以更改运行状态，此时游戏对象处于站立状态（注意速度初始不为0，否则移动不了）。
>
> 如果不全为零，说明至少按下了方向键，此时更改状态机中预设的变量，使其符合跑步状态的值

## 4、物体生成 Instantiate

在游戏场景中生成指定的GameObject

> 制作动画。以制作人物动画为例
>
> 1. unity拖进png图片，选择Multiple，可以使用多张照片，绘制动作。使用Sprite Editor切图合适的大小
> 2. 给每张图片命名方便使用，调整pivot（枢纽点）也就是中心点
> 3. 场景中新建空物体（父物体），把需要的角色素材拖进去（子物体）。选中所有素材，重置位置。调整各个部位的位置。每个素材调整到合适的图层（可以新建图层，人物的各个部件选在这个图层的不同前后位置）
> 4. 选中父物体，也就是人物，新建动画Animation。录制动画，调整形状，绘制动画保存
>
> 这样就有了一个会动的人物，将这个人物拖到Prefab文件夹（已经制作好可以重复使用的游戏对象）里，作为一个预制件，这样方便使用。
>
> 在游戏中如何使用？比如生成刚刚建立的人物对象
>
> ```c#
> //player.cs
> //Instantiate(你希望生成的GameObject，物体的位置，物体的方向);
> //物体的位置（x，y，z）  方向是一组四元数，可以使用Quaternion.Euler(Vector3)来转换Vector3坐标。这样我们可以设置Vector3数值。使用Quaternion.eulerAngles转换为Vector3
> //比如 物体：Enemy，位置：Vector3(20,20,7),方向：Vector3(0,0,90)z轴旋转90°
> 
> public GameObject enemy;
> Vector3 enemyPosition = new Vector3(20,20,7);//位置
> Vector3 enemyRotation = new Vector3(0,0,90);//方向
> 
> //生成游戏对象enemy
> Instantiate(enemy,enemyPosition,Quaternion.Euler(enemyRotation));
> ```

## 5、物体销毁 

炸弹--玩家操作丢物体->几秒后物体爆炸

> 实现炸弹需要：
>
> 1. 控制炸弹出现的位置->玩家按下空格键的位置。那么更改代码->Instantiate(炸弹游戏对象，玩家的位置，物体默认朝向);
>
>    ```c#
>    //player.cs
>    //控制敌人炸弹的生成，是由玩家操作的小人决定，所以这个生成敌人炸弹的代码要挂载在小人Player身上
>    public GameObject enemy;
>    
>    void Update()
>    {
>        if(Input.GetKeyDown(KeyCode.Space))
>        {
>            Instantiate(enemy,transform.position,Quaternion.identity);
>        }
>    }
>    ```
>
> 2. 物体爆炸->物体销毁
>
>    Destroy(希望销毁的物体,销毁的延迟时间秒);
>
>    ```c#
>    //enemy.cs
>    //物品的销毁设定，是物品自己的，这个需要挂载在物品enemy身上
>    public float enemyDeadTime;//敌人死亡延迟时间
>    
>    void Start()
>    {
>    	Destroy(gameObject,enemyDeadTime);//小驼峰的gameObject代表脚本挂载的对象
>    }
>    ```
>
>    更加逼真，添加爆炸特效
>
>    场景新建Effects--Particle System（粒子系统）特效，自定。最后别忘了把粒子效果销毁，少占内存
>
>    ```c#
>    //enemy.cs
>    //敌人生成之后，有一个爆炸效果，爆炸完成后销毁
>    public GameObject enemyDied;
>    
>    void OnDestroy()
>    {
>        Instantiate(enemyDied,transform.position,Quaternion.identity);
>    }
>    ```
>
>    

## 6、循环指令 while/for

在场景中随机生成指定数量的物体

> 素材：石头png图片，拖进prefab文件夹，成为预制件，调整所处图层位置。创建新的游戏对象，挂载脚本
>
> ```c#
> //rock.cs
> public GameObject rock;
> public int rockNumber;
> 
> void Start()
> {
> 	//比如场景开始，随机生成rockNumber个石头
> 	for(int i=0;i<rockNumber;i++)
>     {
>         //Random.range(23,-23)两个数之间随机生成，是视野的尺寸范围内
>         Vector3 randomPosition=new Vector3(Random.range(23,-23),Random.range(10,-10),0);
>         Instantiate(rock,randomPosition,Quaternion.identity);
>     }
> }
> ```

## 7、碰撞检测 OnTriggerEnter2D()

> 刚体 Rigidbody   拥有了物理性质
>
> 碰撞 Collider 有一个isTrigger选项，勾选会变成一个触发器，这代表如果一个物体拥有Rigidbody+Collider接触到了触发器，久会开始执行触发器内部代码--> 比如说，玩家进入敌人视线范围内，这样就需要敌人成为触发器，而不仅仅是玩家碰到敌人才会检测
>
> OnTriggerEnter2D(Collider2D collision)	2D游戏碰撞检测      与Start、Update属于同一种类型，与其他物品重叠时触发

> 随机生成enemyNumber个敌人，在玩家靠近一定范围时，发生爆炸，玩家的血量减少
>
> 玩家player相当于被检测物体，需要有Rigidbody+Collider。而敌人enemy需要变成触发器isTrigger = true
>
> ```c#
> //player.cs
> public int playerHealth = 100;
> ```
>
> ```c#
> //enemy.cs
> //与player发生碰撞时爆炸
> private void OnTriggerEnter2D(Collider2D collision)
> {
>     //玩家掉血
>     collision.gameObject.GetComponent<Player>().playerHealth -= 10;
>     Destory(gameObject);//销毁敌人
> }
> ```

同样的，有炸弹减少player血量，也可以有血包healthPack增加player血量

> 前面的生成敌人、血包，他们的逻辑是类似的，新建一个生成器Spawner脚本
>
> ```c#
> //spawner.cs
> public GameObject enemy;
> public GameObject healthPack;
> public int number;
> public float spawnTime;//生成间隔时间
> public float timeBetween;//记录距离上一次生成的时间
> 
> void Start()
> {
> 	//比如场景开始，随机生成number个敌人
> 	for(int i=0;i<number;i++)
>     {
>         //Random.range(23,-23)两个数之间随机生成，是视野的尺寸范围内
>         Vector3 randomPosition=new Vector3(Random.range(23,-23),Random.range(10,-10),0);
>         Instantiate(enemy,randomPosition,Quaternion.identity);
>     }
> }
> 
> void Update()
> {
>     timeBetween += Time.deltaTime;//每帧timeBetween+=距离上一帧过去的时间
>     //间隔几秒生成一个血包
>     if(timeBetween > spawnTime)
>     {
>         //随机位置生成一个血包
>         Vector3 randomPosition=new Vector3(Random.range(23,-23),Random.range(10,-10),0);
>         Instantiate(healthPack,randomPosition,Quaternion.identity);
>         timeBetween = 0;
>     }
> }
> ```
>
> 血包脚本
>
> ```c#
> //healthPack.cs
> //和玩家进入敌人范围内爆炸类似，但是要确定碰撞体collision是玩家，这就用到tag属性。
> private void OnTriggerEnter2D(Collider2D collision)
> {
>     if(collision.tag == "Player")
>     {
>         //玩家回血
>         collision.gameObject.GetComponent<Player>().playerHealth += 5;
>         Destory(gameObject);//销毁血包
>     }
> }
> ```

## 8、指向移动 MoveTowards()

维度.MoveTowards(内容)

维度 Vector2和Vector3

内容  分为三部分 (被移动的物体，目标位置，速度)  ->  (gameObject.transform.position,new Vector3(0,0,0),5f*Time.deltaTime)

> 比如现在要敌人随机生成在地图的位置，并跟随玩家移动
>
> ```c#
> //enemy.cs
> public float chasingSpeed;//追逐速度
> public GameObject playerObject;//玩家物体,需要与player绑定，但是绑定只能绑定已存在的游戏对象。所以需要在生成器中创建实例，实例再将player绑定，赋予敌人追击目标
> 
> void Update()
> {
>     transform.position = Vector2.MoveTowards(transform.position,playerObject.transform.position,chasingSpeed*Time.deltaTime);
>     
> }
> ```
>
> 之前的生成器代码
>
> ```c#
> //spawner.cs
> public GameObject enemy;
> public GameObject healthPack;
> public int number;
> public float spawnTime;//生成间隔时间
> public float timeBetween;//记录距离上一次生成的时间
> public GameObject playerObject;//玩家物体
> public GameObject newEnemy;//敌人物体
> 
> void Update()
> {
>     timeBetween += Time.deltaTime;//每帧timeBetween+=距离上一帧过去的时间
>     
>     if(timeBetween > spawnTime)
>     {
>         
>         Vector3 randomPosition=new Vector3(Random.range(23,-23),Random.range(10,-10),0);
>         newEnemy = Instantiate(enemy,randomPosition,Quaternion.identity);
>         newEnemy.GetComponent<Enemy>().playerObject=player;
>         Instantiate(healthPack,randomPosition,Quaternion.identity);
>         timeBetween = 0;
>     }
> }
> ```

也存在问题，敌人移动路线一样，会造成堆积在一起

> 解决方案：
>
> 追击速度随时间上升，但还是所有敌人一个速度 --> 引入随机值，随机增加速度，每个敌人速度就不一样了

## 9、函数方法

> 可以将多行代码浓缩出来，重复使用
>
> 比如说，之前的生成器代码，可以抽离出来变成生成函数
>
> ```c#
> //spawner.cs
> public GameObject enemy;
> public GameObject healthPack;
> public int number;
> public float spawnTime;//生成间隔时间
> public float timeBetween;//记录距离上一次生成的时间
> public GameObject playerObject;//玩家物体
> public GameObject newEnemy;//敌人物体
> 
> void Update()
> {
>     timeBetween += Time.deltaTime;//每帧timeBetween+=距离上一帧过去的时间
>     
>     if(timeBetween > spawnTime)
>     {
>         SpawnObject(healthPack);
>         SpawnObject(enemy).GetComponent<Enemy>().playerObject=player;
>         timeBetween = 0;
>     }
> }
> 
> private GameObject SpawnObject(GameObject objectToSpawn)
> {
>     Vector3 randomPosition=new Vector3(Random.range(23,-23),Random.range(10,-10),0);
>     return Instantiate(objectToSpawn,randomPosition,Quaternion.identity);
> }
> ```

## 10、类 class

> 四点改进：
>
> 1. 为游戏场景添加一个阻挡玩家的墙壁
>
>    添加四个方块，加上碰撞体collider
>
> 2. 让摄影机跟随玩家移动
>
>    将摄像机变成玩家的子物体
>
> 3. 确保在unity中可以调试边界的尺寸
>
>    为四个墙壁添加父物体，方便管理，添加脚本
>
>    ```c#
>    //worldbuilder.cs
>    public GameObject wallUp;
>    public GameObject wallDown;
>    public GameObject wallLeft;
>    public GameObject wallRight;
>    //通过更改长宽范围，来更改墙壁的坐标
>    [Range(0,250)]
>    public float verticalRange;
>    [Range(0,250)]
>    public float horizontalRange;
>    
>    private void OnValidate()
>    {
>        //在unity中做出改变，这里面的代码就会执行
>        SetRange();
>    }
>    private void SetRange()
>    {
>        wallUp.transform.position = new Vector3(wallUp.transform.position.x,verticalRange,wallUp.transform.position.z);
>        wallDown.transform.position = new Vector3(wallDown.transform.position.x,-verticalRange,wallDown.transform.position.z);
>        wallLeft.transform.position = new Vector3(-horizontalRange,wallLeft.transform.position.y,wallLeft.transform.position.z);
>        wallRight.transform.position = new Vector3(horizontalRange,wallRight.transform.position.y,wallRight.transform.position.z);
>    }
>    ```
>
>    
>
> 4. 确保敌人和血包始终在地图边界中生成
>
>    ```c#
>    //spawner.cs   
>    //新建一个worldBuilder变量
>    public WorldBuilder worldBuilder;
>    
>    private GameObject SpawnObject(GameObject objectToSpawn)
>    {
>        //修改边界
>        Vector3 randomPosition=new Vector3(Random.range(worldBuilder.horizontalRange,-worldBuilder.horizontalRange),Random.range(worldBuilder.verticalRange,-worldBuilder.verticalRange),0);
>        return Instantiate(objectToSpawn,randomPosition,Quaternion.identity);
>    }
>    ```

class实际上是创建的脚本名，继承一个其他类时，可以使用该脚本的开放变量和开放函数

> 1. 比方，创建一个基类，衍生出几种敌人来继承这个类
>
> ```c#
> //enemy.cs
> 
> public class Enemy : MonoBehaviour
> {
>     public float chasingSpeed;//追逐速度
>     public GameObject playerObject;//敌人追击目标
> 	public int playerDamage;//造成的伤害
>     public GameObject enemydied;//死亡特效
>     
>     public void DoDamge()
>     {
>         playerObject.GetComponent<Player>().playerHealth -= playerDamage;
>         Instantiate(enemydied,randomPosition,Quaternion.identity);
>         Destory(gameObject);//销毁敌人
>     }
>     
>     public void MoveToPlayer()
>     {
>         transform.position = Vector2.MoveTowards(transform.position,playerObject.transform.position,chasingSpeed*Time.deltaTime);
>         chasingSpeed += Random.Range(0.001f,0.003f);
> 
>     }
> }
> 
> ```
>
> 建立几种敌人类型，继承这个Enemy
>
> ```c#
> //EnemyNormal.cs
> //追逐玩家，逐渐提升速度；对碰到的玩家造成伤害；碰到玩家后自爆
> public class EnemyNormal : Enemy
> {
>     //扣除玩家生命值，并自爆
>     private void OnTriggerEnter2D(Collider2D collision)
>     {
>         if(collision.gameObject == playerObject)
>         {
>             DoDamge();
>         }
>     }
>     //驱动敌人追逐玩家
>     void Update()
>     {
>         MoveToPlayer();
>     }
> }
> ```
>
> ```c#
> //EnemySummoner.cs
> //出生后原定不动；被玩家碰到后自爆；定期生成普通敌人；死亡时帮助玩家回血
> public class EnemySummoner : Enemy
> {
>     public Spawner spwaner;//2.创建使用（类）变量，并在unity绑定
>     public float timeBetween;
>     
>     //生成敌人，碰到玩家自爆
>     private void OnTriggerEnter2D(Collider2D collision)
>     {
>         
>         
>         if(collision.gameObject == playerObject)
>         {
>             DoDamge();
>         }
>     }
>     
>     void Update()
>     {
>         timeBetween += Time.deltaTime;
>     
>         if(timeBetween > 3)
>         {
>             spwaner.SpawnObject(spwaner.enemy,gameObject.transform.position).GetComponent<Enemy>().playerObject=spwaner.player;
>             timeBetween = 0;
>         }
>        
>     }
> }
> ```
>
> ```c#
> //EnemyBomber.cs
> //出生后在原地不动，在玩家触碰时造成大量伤害并自爆
> public class EnemySummoner : Enemy
> {
>     private void OnTriggerEnter2D(Collider2D collision)
>     {
>         if(collision.gameObject == playerObject)
>         {
>             DoDamge();
>         }
>     }
>     
> }
> ```
>
> 3.创建类变量后，使用构造函数赋值
>
> 比如Vector3 
>
> ```c#
> public Vector3(float x,float y,float z)
> {
>     this.x=x;
>     this.y=y;
>     
> }
> ```
>
> 

