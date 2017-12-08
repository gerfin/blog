# 总体架构
> - 前台
> 	- 文章模块
> 	- 用户模块
> 	- 评论模块
> 	- 赞模块
> 	- 搜索模块
> 	- 个人中心模块
> - 后台
> 	- 后台架构
> 	- 管理人员模块
> 	- 审核模块
> 	- 权限模块
> 	- 专题管理
> 	- 系统通知模块

## laravel5.5的安装与启动

### composer的安装
- 自动安装：https://getcomposer.org/download/提供了自动安装的方法
- 手动安装：
- 在上述网站中的Manual Download列表下载最新的composer.phar的文件
 - linux系统：将该文件拷贝到/usr/bin/composer文件夹下（cp composer.phar /usr/bin/composer），然后赋予执行权限（chmod +x /usr/bin/composer），运行composer即可显示
 - windows系统：找到php目录，将composer.phar放在php根目录下，同时新建bat文件，文件内容为： <code>@ECHO OFF
 php "%~dp0composer.phar" %*</code> 
 运行dos下运行composer即可
使用中国镜像服务器:<code>composer config -g repo.packagist composer https://packagist.phpcomposer.com</code>
- composer运行成功界面
![composer](composer.png)
---

### laravel5.5的安装

- 在站点目录下运行composer命令：<code>composer create-project --prefer-dist laravel/laravel laravel55 </code>，其中最后关键字laravel55即为laravel安装的程序目录，相关安装文档见[laravel中文安装文档](https://d.laravel-china.org/docs/5.4/installation)
- 推荐使用[phpstorm](https://www.jetbrains.com/zh/phpstorm/)进行代码编辑
- 启动laravel:使用laravel自带的artisan命令即可启动php站点,进入上一步创建的laravel程序目录laravel55，运行artisan命令（在laravel下启动终端）：php artisan serve,站点即启动，访问http://127.0.0.1:8000 即可看到laravel的站点![laravel-page](laravel-page.png)
- laravel目录结构介绍见[laravel中文-Laravel 的文件夹结构](https://d.laravel-china.org/docs/5.4/structure)
- 配置文件的修改
	- 配置文件位于laravel55/.env,如果没有，将laravel55/.env.example改成.env
	- 根据自己的数据库相关情况配置，例如我的将laravel用到的数据放在homestead数据库中，我的配置如下
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=[mysql的用户名]
DB_PASSWORD=[mysql的密码]
<i>在向github更新版本时，不要将.env更新上去，里面包含你的相关配置信息</i>
---
## 文章模块的设计
### 文章模块架构
> - 路由
> 	- 路由
> 	- 模板
> - 表设计
> 	- 表设计
> 	- 模型
> - 页面逻辑
> 	- 文章列表
> 	- 添加文章
> 	- 编辑文章
> 	- 删除文章
> 	- 文章详情

<i>用phpstorm打开该项目文件夹(laravel55)</i>
### 路由
路由的相关文档见[laravel中文文档-Laravel HTTP 路由功能](https://d.laravel-china.org/docs/5.4/routing)
<i>此项目的路由配置表位于laravel55/routes/web.php</i>
#### 文章路由结构
> - 文章列表
> - 添加文章
> - 编辑文章
> - 删除文章
> - 文章详情

#### 文章模块的具体设计
- 新建文章列表页路由并指向PostController下的index方法，具体为在web.php写下如下代码:

```php
//文章列表页
Route::get('/posts','\App\Http\Controllers\PostController@index');
//文章详情页
Route::get('/posts/create','\App\Http\Controllers\PostController@create');
Route::get('/posts/{post}','\App\Http\Controllers\PostController@show');
//创建文章
Route::post('/posts','\App\Http\Controllers\PostController@store');
//编辑文章
Route::get('/posts/{post}/edit','\App\Http\Controllers\PostController@edit');
Route::put('/posts/{post}','\App\Http\Controllers\PostController@update');
//删除文章
Route::get('/posts/{post}/delete','\App\Http\Controllers\PostController@delete');
```

- 在app/Http/Controller下创建PostController控制器，可直接使用artisan命令：<code>php artisan make:controller PostController</code>
- 在PostController添加路由中指定的function，具体代码如下：

```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class PostController extends Controller
{
    //列表
    public function index()
    {

    }

    //文章详情页面
    public function show()
    {

    }

    //创建文章页面
    public function create()
    {

    }

    //创建文章逻辑
    public function store()
    {

    }

    //编辑文章页面
    public function edit()
    {

    }

    //编辑文章逻辑
    public function update()
    {

    }

    //删除文章逻辑
    public function delete()
    {

    }
}
```

- blade模板渲染文章页面
> <b>需要渲染的文章页面</b>
> - 创建文章页面
> - 编辑文章页面
> - 文章列表页面
> - 文章详情页面

	- 文章渲染需要使用view函数，在文章列表页中的具体实现为：
	```php
    //列表
    public function index()
    {
        return view("post/index");
    }
    ```
    <i>return的view函数是在laravel55/resources/views下去找模板，如view("post/index")是在post文件夹下找index.blade.php,对于页面渲染的相关文档，参照[laravel中文文档-Laravel 的视图功能](https://d.laravel-china.org/docs/5.4/views)</i>
    - 在laravel55/resources/views/下新建post文件夹,在post文件夹下，新建index.blade.php,将html的内容复制进去，该文件位于[课程材料/前台/文章列表.html](课程材料/前台/文章列表.html),将[课程材料/前台/](课程材料/前台/)下的css,front,js,image文件夹拷贝到项目中laravel55/public文件夹下，将刚才的index.blade.php中所有的相对路径修改为绝对路径(目的是为了正确引用public文件夹下的js，css文件)，如(注意href后的变化)：
    ```php
    <link href="css/blog.css" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="css/wangEditor.min.css">
    ```
    修改为=>
    ```php
        <link href="/css/blog.css" rel="stylesheet">
        <link rel="stylesheet" type="text/css" href="</css/wangEditor.min.css">
    ```
- 更新PostController，具体代码如下:
```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
    class PostController extends Controller
    {
        //列表
        public function index()
        {
            return view("post/index");
        }

        //文章详情页面
        public function show()
        {
            return view("post/show");
        }

        //创建文章页面
        public function create()
        {
            return view("post/create");
        }

        //创建文章逻辑
        public function store()
        {

        }

        //编辑文章页面
        public function edit()
        {
            return view("post/edit");
        }

        //编辑文章逻辑
        public function update()
        {

        }

        //删除文章逻辑
        public function delete()
        {

        }
    }
```
| view	 	| blade模板名 | html文件目录|
|-----------|------------|------------|
| view("post/index")|index.blade.php| [课程材料/前台/文章列表.html](课程材料/前台/文章列表.html)|
|view("post/show")|show.blade.php| [课程材料/前台/文章详情页.html](课程材料/前台/文章详情页.html)|
|view("post/create")|create.blade.php|[课程材料/前台/文章创建.html](课程材料/前台/文章创建.html)|
|view("post/edit")|edit.blade.php|[课程材料/前台/文章编辑.html](课程材料/前台/文章编辑.html)|
<i>注：需要在laravel55/resources/views/下创建文件夹，并创建对应的*.balde.php,将对应的html内容复制进去</i>
- 使用laravel blade调整html页面
	- 提取公用的html模块
	- 此部分请参照[laravel中文文档-Laravel 的视图功能](https://d.laravel-china.org/docs/5.4/views)
	- 在laravel55/resources/views/下创建layout文件夹
	- 将上节中的代码进行了分割，具体见源代码
- 文章模块的数据表
> 该部分数据迁移相关内容请参照[laravel中文文档-Laravel 的数据库迁移](https://d.laravel-china.org/docs/5.4/migrations)

	- 数据迁移表的建立，使用artisan命令:<code>php artisan make:migration create_posts_table</code>,在项目的app/database/migration下生成了<i>*_create_posts_table.php</i>,在*_create_posts_table.php文章模块所需要的字段,配置好的代码如下:
	```php
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
class CreatePostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title')->default("");
            $table->text('content');
            $table->integer('user_id')->default(0);
            $table->timestamps();

        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('posts');
    }
}
```
	- 配置好该迁移表后，运行artisan的迁移命令，向数据库中生产数据表结构(php artisan migrate)，mysql中产生了以上的数据表结构

- 文章模型创建
> 该部分所用的文章模型参照[laravel中文文档-Eloquent](https://d.laravel-china.org/docs/5.4/eloquent)

	- 使用artisan命令php artisan make:mode Post将在laravel55/app下产生Post.php,该model自动与posts关联，可以通过继承自model类中的<code>protect $table</code>字段进行修改
	- 使用tinker产生测试数据
		- 通过php artisan tinker 启动tinker
		- 如果想产生post单条测试数据，可以在thinker下
		```shell
        $post = new \App\Post()
        $post->title = 'title test'
        $post->content = 'content test'
        $post->save()
        ```
        ![tinker](tinker.png)
        在数据库中我们可以查看posts表下新增加了数据，但是数据表的created_at和updated_at字段的时间与当前时间不符，需要修改laravel的默认时区
        ![post数据](post数据.png)
        - 时区设置：在config/app.php中设置时区,其中有个<code> 'timezone' => 'UTC',</code>字段,需要将时区更改为<code> 'timezone' => 'Asia/Shanghai'</code>
- 文章列表页的实现
	- 对PostController下的index方法进行修改：
	```php
    //文件头部分要添加use \App\Post
    public function index()
    {
    //从Post模型中取出数据按照创建时间降序排列
        $posts = Post::orderBy('created_at','desc')->get();
    //将页面及上步取出的数据返回给前端blade页面
        return view("post/index",compact('posts'));
    }
    ```
    - 对resources/views/post/index.blade.php进行修改
    > 修改后的代码如下

```php
@extends("layout.main")
@section("content")
<div class="col-sm-8 blog-main">
                <div>
                    <div id="carousel-example" class="carousel slide" data-ride="carousel">
                        <!-- Indicators -->
                        <ol class="carousel-indicators">
                            <li data-target="#carousel-example" data-slide-to="0" class="active"></li>
                            <li data-target="#carousel-example" data-slide-to="1"></li>
                            <li data-target="#carousel-example" data-slide-to="2"></li>
                        </ol><!-- Wrapper for slides -->
                        <div class="carousel-inner">
                            <div class="item active">
                                <img src="http://ww1.sinaimg.cn/large/44287191gw1excbq6tb3rj21400migrz.jpg" alt="..." />
                                <div class="carousel-caption">...</div>
                            </div>
                            <div class="item">
                                <img src="http://ww3.sinaimg.cn/large/44287191gw1excbq5iwm6j21400min3o.jpg" alt="..." />
                                <div class="carousel-caption">...</div>
                            </div>
                            <div class="item">
                                <img src="http://ww2.sinaimg.cn/large/44287191gw1excbq4kx57j21400migs4.jpg" alt="..." />
                                <div class="carousel-caption">...</div>
                            </div>
                        </div>
                        <!-- Controls -->
                        <a class="left carousel-control" href="#carousel-example" data-slide="prev">
                            <span class="glyphicon glyphicon-chevron-left"></span></a>
                        <a class="right carousel-control" href="#carousel-example" data-slide="next">
                            <span class="glyphicon glyphicon-chevron-right"></span></a>
                    </div>
                </div>        <div style="height: 20px;">
                </div>
                <div>
                    @foreach($posts as $post)
                    <div class="blog-post">
                        <h2 class="blog-post-title"><a href="/posts/{{$post->id}}" >{{$post->title}}</a></h2>
                        <p class="blog-post-meta">{{$post->created_at->toFormattedDateString()}} by <a href="/user/5">Kassandra Ankunding2</a></p>
                        <p>{{$post->content}}
                        <p class="blog-post-meta">赞 0  | 评论 0</p>
                    </div>
                    @endforeach
                    <ul class="pagination">
                        <li class="disabled"><span>&laquo;</span></li>
                        <li class="active"><span>1</span></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=2">2</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=3">3</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=4">4</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=5">5</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=6">6</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=7">7</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=8">8</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=9">9</a></li>
                        <li><a href="http://127.0.0.1:8000/posts?page=10">10</a></li>


                        <li><a href="http://127.0.0.1:8000/posts?page=2" rel="next">&raquo;</a></li>
                    </ul>

                </div><!-- /.blog-main -->
            </div>
@endsection
```
- 数据填充（产生大量文章测试数据）
	- 数据填充配置目录位于databases/factories/UserFactory.php中，添加如下代码:
	```php
    $factory->define(App\Post::class,function (Faker\Generator $faker) {

    return [
        'title' => $faker->sentence(6),
        'content' => $faker->paragraph(10),
    ];
});
```
	- 进入tinker:php artisan tinker
	- <code>factory(App\Post::class,10)->make()</code>产出10条随机数据，但是数据不保存到数据库中，factory(App\Post::class,10)->create()产生20条数据，并且保存到数据库中


