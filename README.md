# Laravel-Memories
Laravel Notes about study/development and most current issues and commands

**Project in Laravel**
--
*create a project in Laravel: composer create-project --prefer-dist laravel/laravel blog*
> ext .app and .dev -> .test
> changed the "LocalHost" for another name, not doing it, if has some issue get back here *********

*Folder Inside
>HTTP/"Where put the routes and security"
>config/app -> install packges log
>database.php-> database config
>public-> css and js files
>vendor->package install keep located

*Routes Examples
Route::get('/', function () {
    return view('welcome');
});

Route::get('/about', function () {
    return "This is the about page";
});

Route::get('/contact', function () {
    return "This is the contact page";
});

>Pass variables through the url

Route::get('/post/{id}', function ($id) {
    return "This is the post $id page";
});

//Create an array that associate the url in 1° param to admin.home and return the Url
//Nickname it the url (dar apelido a URL)
Route::get('admin/posts/example', array('as'=>'admin.home' ,function () {

    $url = route('admin.home');

    return "This url is" . $url;


}));

>namespace take off the conflict for using the same function/class name
>Creating a controller from cmd:

php artisan make:controller <NAMEOFTHECONTROLLER>

>Create a controller and access controller class and the method in it:
>Can pass an variable through the function method 

Route::get('/pagename/{id}', NameOfTheController@nameOFtheMEthod

NameOFtheController extends Controller

nameOFtheMEthod($id){

	return "SOMETHIN PLEASE" $id!;
}

*Comando to create CRUD class
php artisan make:controller --resource <NAMEOFTHECONTROLLER>

*Routes with basic CRUD class
Route::resource('/post', 'PostsController');
with: php artisan route:list, we can see that this 'resource' create already all CRUD functionalities

|        | GET|HEAD  | posts               | posts.index   | App\Http\Controllers\PostsControlle
r@index   | web          |
|        | POST      | posts               | posts.store   | App\Http\Controllers\PostsControlle
r@store   | web          |
|        | GET|HEAD  | posts/create        | posts.create  | App\Http\Controllers\PostsControlle
r@create  | web          |
|        | GET|HEAD  | posts/{post}        | posts.show    | App\Http\Controllers\PostsControlle
r@show    | web          |
|        | PUT|PATCH | posts/{post}        | posts.update  | App\Http\Controllers\PostsControlle
r@update  | web          |
|        | DELETE    | posts/{post}        | posts.destroy | App\Http\Controllers\PostsControlle
r@destroy | web          |
|        | GET|HEAD  | posts/{post}/edit   | posts.edit    | App\Http\Controllers\PostsControlle

>Views folde in route if not using controller or in Controller, has to Return the folde and the file

return view('outside/fale-conosco');

>Pass data to the return

return view ('post')->with('id',$id);

using the with assume that when the view 'post' show up the id variable is requested and return to the HTML file

or can use it the compact function that allow you to pass multiples paramters in a single funtion

return view ('post', compact('id', 'name', 'password'));

>@yeild('content') 

use it on layout file for create dynamic pages and use it less resources on the html code or scripts

>@extends('FILE/foldername or file.foldername)

use to reproduce the layout file in others pages

>@section('content') with @endsection or @section wtih @stop

use in combination with the @yield to determine what is going to be show up in view

>@if(condition) equal to if statement condition but have to be closed with @endif

>@foreach($array as $variable)

 statements

@endforeach

>@include equal to include or require those last are diferent in showing page but both INCLUDE files to actual page

>.env files area set up of your project
>database.php in config folder has an array with the most used database connections

>after set your database enviroments you can use the 

'php artisan migrations' to create tables in your database with some default colums

>to create a class to after migrate the table in phpmyadmim

you use it the comand

php artisan make:migration create_posts_table --create="TABLENAME"

>in class you can use the comand ->unique for reserve the term

inside the class you have a blueprint (FOR more SEARCH for DEPENDENCIES INJECTIONS)

example object: $table->TYPEOFVARIABLE('VARIABLENAME')->FUNCTIONS(IF You want it);

>(Solution)Got some problems with migrate, insert into AppServiceProvider.php

use Illuminate\Support\Facades\Schema;

public function boot()
    {
        Schema::defaultStringLength(150);
    }

then use it the php artisan migrate:fresh


>php artisan migrate:rollback 

This comand undo the last migrate that you coded for

>To add a colum to existing tables

php artisan make:migration Name_of_the_Class --table="nameOFtheTABLE"

Schema::table('posts', function (Blueprint $table) TO insert a table
Schema::create('posts', function (Blueprint $table) TO create a table

>php artisan migrate:refresh 

do a rollback and a migrate after

>php artisan migrate:reset

do a rollback and empty all database left only the migrations table
>php artisan migrate:status

give you the status if the tables already migrate

>INSERT data into DB
 
Route::get('/insert', 'PostsController@insert');
Every time that the /insert page
DB::insert('INSERT INTO posts (title, body) VALUES (?, ?)', ['PHP with Laravel', 'Laravel GodLike']);

>READ data from DB

Route:get('/read', 'PostsController@read');
DB:select('SELECT * FROM posts WHERE id=?', [1]);

To show in the VIEW

$results = DB:select('SELECT * FROM posts WHERE id=?', [1]);

foreach ($results as $result){

$title = $result->title
$body = $result->body

return $title . " and " . $body;

}

>UPDATE data from DB

Route:get('/update', 'PostsController@update');

$update = DB::update('UPDATE post SET title="Title where sucessfully updated" WHERE id=?', [1]);

Return $update;

>DELETE data from DB

Route:get('/delete' , 'PostsControllers@delete');

*Got an error on the SQL i wroted = DELETE posts WHERE, supressing the FROM 
$deleted = DB::delete('DELETE FROM posts WHERE id = ?', [1]);

return $deleted;

************************************
ELOQUENT DB MANIPULATION
************************************
>Creating a model with the command

php artisan make:model <NAMEOFTHEMODEL>

i left the model empty

Route:get('/read-db', 'PostsController@readDb');

function public readDb() {

//App is an static method and has to used it :: to be access
//the all() function get all the record from the database from the model App
//and store in the $posts var

$posts = App::all();  

or to a specific row

$posts = App::find(ID); //Retrieve data using the PRIMARYKEY

to access it 

foreach($posts as $post){

return $post->title;

>Find data into the DB

Route::get('/find', 'PostsController@find');

public function find(){

$posts = Posts::Where('id', 5)->orderBy('id', 'desc')->take(2)->get();

Functions:

******Where() ->
- First param = Colum Name
- Second Param = Operators (= or > or <, suported by the DB);
- Third Param = Values 
	If you want to test it a equal statement, just pass the
Two params like the example above.
*****************

******orderBy()->
- First param = Colum Name that you wish to be sorted by
- Second Param = Controls the direction of the sort (DESC or SORT);
*****************

******take()->
- First param = limit the number of results
*****************

******get()->
- Firts param = generally keep with a blank space
use it to retrive information from database
******************

> Retrieve data more functions

$post = Posts::where('id', '>', 4)->findOrFail();

******findOrFail()->
- First param = generally keep it with a blank space
use it to retrive data or if not founded die or show try catch page
*****************

******firstOrFail()->
- First param = generally keep it with a blank space
use to select the first row in the params setup usually with WHERE function
*****************

>Insert Data

//One Way
$post = new Posts;

$post->title = "SOME TEXT WAY 2 CREATIVE";
$post->body="SOME TEXT WAY too CREATIVE";

$post->save(); 

//Second Way + Update one row

$post = Post::find(PRIMARY--KEY)

$post->title = "CHEAP CHAT";
$post->body="SOMETEXT";

$post->save();

*****save()->
Save the information to Posts MODEL and insert into DB
******************

>Create data to DB

Route::get('/create', 'PostsController@create');

in the class Posts:

protected fillable = ['title',
	               'body];

$post = Posts::create(['title'=>'I\'m Changing the Content',
		'body=>'I am Changing the body']);

**YOU CAN'T create info into DB withou acess a protected atributte in this case


>Update data to DB

$post =Posts::where('id', 4)
	->where('is_admin', 0)
	->update(['title'=>'Message',
	                 'body'=>'Message2']);

**********update()->
The update method allow us to use arrays to update the DB and
use it another functions like WHERE to constraints the range
******************

>DELETE data to DB

//Can delete the data from DB with
$post = Posts::find(3);

$post->delete( );

**or

Posts::destroy(4,5);

***or

Posts::where('is_admin', 0)->delete();

*******delete()->
Delete data from DB generally dont use params
***************

*******destroy()->
you can pass multiples param
****************

						
>INSERT a column into a database table

to insert delete_at TIMESTAMP into posts TABLE

php artisan make:migration add_deleted_at_column_at_posts_table --table=posts

Create a Class

To use pre determined timestamps use it SoftDeletete that give you the time
that the data was delete from the DB

in the Class we use Schema::table to insert a table

instanciate with $table->softDeletes( );
Softdeletes will bring "deleted_at" column to add to your table

can use in down( ) function, the function dropColumn('deleted_at');

> Retrieve time that DATA  was deleted

$post = Posts::whithTrashed( )
	   ->where( 'id', 1)
	   ->get( );

or for more than one data info **

$post = Posts::onlyTrashed( )
	    ->where('is_admin', 0)
	    ->get( );

********withTrashed( ) and OnlyTrashed( )
Generally comes with a blank space, is used to group it the SoftDelete items,
basic to detect then.
******************

>Restore a SoftDeleted Data from DB

to restore you can use a restore function

$post = Posts::onlyTrashed( )
	   ->where('is_admin', 0)
	   ->restore( );

>Delete a DATA permanently

$post = Posts::onlyTrashed( )
	->where('id', 1)
	->forceDelete( );

*****forceDelete( )
Generally comes with a blank space
use it to delete it from DB without TimeStamp
****************

/************************************************** */
    //ELOQUENT Relations
/************************************************** */

>ONE TO ONE

>On the Users Model (Father)

Create a fucntion call

public function NameOFtheFunctionInTheModel( ){

return $this ->hasOne('App\Posts');

}

*******hasOne( )
First Param - The model
Second Param - The Column of the Table
Third Param - IDK Yet.
****************

Create a Route

Route::get('/userpost/{id}/post', 'PostsController@userPost');

Create Function

public function userPost( ){

//You can access all columns table like objects
//become more easier to manipulate DB information

return User::find( 1 ) -> NameOFtheFunctionInTheModel -> title;

> ONE TO ONE INVERSION

Same Process Differeing on:

On the Post Model

Create a function 

public function NameOFtheFunctionInTheModel( ){

	return $this->belongsTo('App\User');

********belongsTo( )
Same Param that the hasOne function, but this function
is used to see where that info come from
*******************

Create a route

Route::get('/postuser/{id}/user', 'PostsController@postUser');

public function postUser($id){

return  Post::find($id)->NameOFtheFunctionInTheModel->name;

>TIPS

When Commands on migrate doesnt working
Just delete the tables on the phpmyadmim or the program that 
you are using to See your DB.


>ONE TO MANY

*Model
public function postMany( ){

return $this->hasMany('App\Posts');


*Route:
Route::get('/userpostmany ', 'PostsController@userPostMany);

*PostController

public function userPostMany( ){

$user = User::find(1);

foreach($user->userPostMany as $post){

echo $post;

}

>MANY TO MANY

************************************************************
Created two models the roles and role_user*
*role_user, if we want to relations table we have to create another table
with both names separates by a underline, but the order has to be alphabetic
or else you have to changed in the model like this:

return $this->belongstoMany('NameOfTheModel', 
		             'Unity_tables_name',
		             'Foreign Key #1',
		             'Foreign Key #2'); 

Both has the same effect;
*************************************************************

*Model in User
public function manyRolesMany( ){

return $this->belongsToMany('App\Role');

*Route:
Route::get('/manyrolemany/{id}/role ', 'PostsController@manyRoleMany);

*PostController

public function manyRoleMany($id){

$user = User::find($id);

foreach($user->manyRoleMany as $role){

retrun $role->name

}

***PROBLEM in this area*********
for somehow i tried to make the same thing without using foreach
HAVE TO SEE THIS PART WHY!!!!!!
*******************************


>QUERYING INTERMEDIATE TABLE

*Model in User
public function intermediateRole( ){

return $this->belongsToMany('App\Role')->withPivot('created_at');

*****withPivot( )
- Multiples Param - name of the columns
Retrieve the info data from de PIVOT(INTERSECTION TABLE)
**************************



*Route:
Route::get('/intermediaterole ', 'PostsController@interRole);

*PostController

public function interRole( ){

$user = User::find( 1 );

foreach($user->intermediateRole as $role){

echo $role->pivot->created_at;

}

>HAS MANY THROUGH


*Model
public function getWhereFromPost( ){ 

return $this->hasManyThrough('App\Posts', 'App\User');

*****hasManyThrough( )
First Param - Model #1
Second Param - Model #2
Third Param - Column with id, if its not set with the convetional way
Fourthy Param - Column with id Name of Second Model 
***************************

*Route:
Route::get('/hasmanythrough', 'PostsController@hasManyThrough);

*PostController

public function hasManyThrough( ){

$country = Country::find(1); //country_id
***************************
**PS:When i'm relating those models with some functions like Find( )
inside those functions is the id from the model, so if i'm using User model
inside my function find its user_id.

It was like was this way, but it is in default so after the second param
those keys are supressed
return $this->hasManyThrough('App\Post', 'App\User', 'country_id', 'user_id');
***************************

foreach($country->hasManyThrough as $post){

return $post->title;

}

>POLIMORPHIC RELATION

**TIPS

php artisan make:model Photo -m, this -m migrated already the model
And Create a Model and a Migration table

LARAVEL DOCS DEFINITION
A polymorphic relationship allows the target model to belong 
to more than one type of model using a single association.
Using a one-to-one polymorphic relation allows you to have a single list 
of unique images that are used for both blog posts and user accounts

****************************
*Command return just one value* 
****************************

****************
**On the Models**
****************

**Photos

public function imageable( ) {

         return $this->morphTo( );

}

**User 
public function photos( ) {


return $this->morphToMany('App\Photos', 'imageable');

}

**Posts
public function photos( ) {


return $this->morphToMany('App\Photos', 'imageable');

}

**Routes

Routes::get('/polimorphicRel', 'PostsController@polimorphicsRelations

**if you want to the retrieve data from photo using the user_id call the 
User:: or using the Posts Posts::

public function polimorphicsRelations( ){

$user = User::find(1); //user_id;

foreach($user->photos as $photo){

    return $photo->path; //Just return one VALUE

 }
}

>POLIMORPHICS INVERSIONS

**Photos Model

public function imageable( ){

return $this->morphTo( );

}

**Route

Route::get('/polimorphicsInversion', 'PostsController@polimorphicsInversion');

**PostsController

public function polimorphicsInversion( ){

$user = Photos::findOrFail(1);

return $user->imageable;


}

**********************
***ON MIGRATIONS***
**********************

tag_id = it's just the id of the tag
taggable_id = is where the videos id go
taggable_type = is where the 'Models' come like: 'App\Videos'

***********************
*****TINKER**********
***********************

Tinker can interact with the DB withou changes the main code

Enter in TINKER painel

php artisan tinker
>>> SHOW THIS three ARROWS to DETERMINE that you are in Tinker

**CREATE

$post = new App\Posts;

$post->title ="CrEATE SOMENthing";

Than $post->save( );

**READ

$post = App\Posts::find(1);

$post->title;

**UPDATE

$post = App\Posts::find(1);

$post->title = "CHANGE THE CONTENT";

$post->save( );

**DELETE

$post = App\Posts::find(1);

$post->delete( ); //This is just a softDelete

$post->forceDelete( );

********************************************************
******DB ELOQUENT ONE to ONE Relations CRUD**********
********************************************************

Create a new Project One to One, Change de PRoviders Service in App

>Create a model

**
php artisan make:model Address -m (This way you createa model and migrated at the same time)

**Database
When create a migrations

$table->integer('user_id')->unsigned();

unsigned means all positive values

**Model
Read like this(if helps):" The User hasOne Address"

public function address( ){
	
  return $this->hasOne('App\Address');
	

}

**Class
Import Class like this:

use App\User;
use App\Address;

**Route

Route::get('/insert', function( ){

$user = User::findOrFail(1);

$address = new Address(['name'=>'1234 Houston NY']);

$user->address( )->($address);

address( ) Call the Method from Model Address

});

*******************************
*****CRUD APPLICATIONS******
*******************************

***CREATE
**Routes

Route::resources('/postsCRUD', 'PostsController');

This Createthe basics DB manipulations

see in => php artisan route:list

the method is define for each action that ypu want to do
if a form is gonna take information(STORE) we use the method
POST that is already define in route:list

**Model =>User

public function posts( ){

     return $this->hasMany('App\Posts');

}

**Views
@extends ('Layouts.app) //Folder App/Layouts

@section('content')
***************OBS:***************
<form method="Method in route:list" action="URL in route:list">
**********************************
<form method="POST" action="/postsCRUD">
	<input type="submit" name="submit" placeholder="Enter Title">
	<input type="text" name="submit">

@endsection

@yield('footer');


**Controller

public function create( ){

return view('postsCRUD.create');

}

************OBS***************
**return view('FolderThatTheViewIsLocated.create');**
*******************************

When creating something in resources model and retrieve information
to DB using a static method, i have a issue with strict on database

config/database.php, in MySQL Array look for strict and turn it to 'false'

***READ

Same process difering by the Controller

When callin the static class return redirect to send de user for another page

Return rediret('/contact'); => ('NAMEofTHEpage');



//Retrieve info to DB  

//1° way
//Do not fill DB with info, just return the info to the screen
return $request->all( );

//2° Way
Access the Posts DB and create a info
Posts::create($request->all( ));

//3°Way
$input = $request->all( ); **$input var become an array
$input['title'] = $request->title; **$input access only title
Posts::create($request->all( ));

//4°Way
$post =new Posts;
$post->title = $request->title; //Request from Create Method
$post->save( );

**have to request the data, it is a function parameter

********************************
****ROUTE BUTTONS***********
********************************

Route::get('/nameOftheURL', 'Controllers@index')->name('nameRELATEDtoHTML');

HTML
<a href="{{route('nameRELATEDtoHTML')}}>

****OBS
To remove Login Auth files
auth/login.blade.php
auth/register.blade.php
auth/passwords/email.blade.php
auth/passwords/reset.blade.php
layouts/app.blade.php
home.blade.php

****Multiple Users
Config/Auth
providers' => [
    'users'  => [
        'driver' => 'eloquent',
        'model'  => App\User::class,
    ],
    'managers'  => [
        'driver' => 'eloquent',
        'model'  => App\Manager::class,
    ],
    'admins'  => [
        'driver' => 'eloquent',
        'model'  => App\Admin::class,
    ]
]

*****Autheticate
Auth::attempt($credentials) // use default guard for simple users
Auth::guard('manager')->attempt($credentials)
Auth::guard('admin')->attempt($credentials)


********************************
**********MiddleWare ***********
********************************


