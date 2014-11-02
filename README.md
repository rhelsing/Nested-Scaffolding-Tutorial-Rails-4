#Nested Scaffolding Rails 4
---

Working sample of nested scaffold with Users and Posts

##Steps used to make project:
---

* rails g scaffold user name:string
* rails g scaffold post title:string body:string user:references
* Edit post migration: t.references :user >> t.belongs_to :user (optional?)
* Edit routes.rb

```ruby
resources :posts
resources :users
```

becomes:

```ruby
resources :users do
  resources :posts
end
```

* rake db:migrate
* Edit post controller

```ruby
#controllers/user.rb#index
@posts = User.find(params[:user_id]).posts
#controllers/user.rb#index
@user = User.find(params[:user_id])
#controllers/user.rb#create
@post = Post.new(:user_id => @user.id)#need to specify ahead of time
#controllers/user.rb#create, update
redirect_to user_post_path(@post.user.id, @post.id) on create, update
#controllers/user.rb#delete
redirect_to user_posts_path(@post.user.id) on delete
```

* Edit post/_form : form_for [@user, @post]

```erb
<%= f.hidden_field :user_id %>
```

* Show and Back links on edit, new, and show
* Show, edit, delete, new links on index
* Posts link on users index
* rails s and test


## Todo
* create nested generator

```command
rails g nest user name:string && user:has_many:post title:string
rails g nest user name:string has_many && user:has_many:post title:string body:text && post:has_many:comment body:text && user:has_many:notes content:text
```

* goal is to generate complicated associations with one command.
* also has_one / has_many_and_belongs_to
