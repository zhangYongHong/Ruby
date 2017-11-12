# Ruby

第一次作业和第二次作业合在了一起.本次作业写的是一个博客，实现功能增删改查的功能

* 新建工程

      rails new blog_of_vito
      
* 打开工程，新建scaffold（脚手架）
       
      rails g scaffold article
      
  
  ![出现以下提示](https://github.com/zhangYongHong/Ruby/blob/master/Images/%E6%93%8D%E4%BD%9C%E8%BF%87%E7%A8%8B2.png)

>  短短一行代码，便为我们创建了这么多的功能，其中包括model层（M）、view层（V）和controller层（C），以及迁移（migration）、路由（routing）、helper方法、测试功能等其他一些方法

> blog需要有题目和内容，我们需要在Article模型对应的表上添加这两个字段

* 打开/db/migrate/*_create_articles.rb文件，修改为：
            
            def change  
                   create_table :articles do |t|    
                   t.string :title    
                  t.text :text    
                  t.timestamps  
                  end
            end
* 运行迁移：
                 
             rails db:migrate
            
* 修改articles_controller.rb文件
            
            def article_params
                  params.require(:article).permit(:title, :text)
            end
            
* 修改view文件

       1.修改/app/view/articles/_form.html.erb文件为：

         <%= form_for @article do |f| %>  
         <% if @article.errors.any? %>
            <div id="error_explanation">
            <h2><%= pluralize(@article.errors.count, "error") %> prohibited this article from being saved：</h2>
                  <ul>
                   <% @article.errors.full_messages.each do |message| %>
                   <li><%= message %></li>
                   <% end %>
            </ul>
            </div>
       <% end %>

      <p>
             <%= f.label :title %><br>
             <%= f.text_field :title %>
      </p>
      <p>
        <%= f.label :text %><br>
        <%= f.text_area :text %>
      </p>
      <p>
            <%= f.submit %>
      </p>
      <% end %>
      
      
      2.修改index.html.erb文件为：
      
      <p id="notice"><%= notice %></p>
            <h1>Articles</h1>
            <table>
                  <thead>
                        <tr>
                              <th>Title</th><!--new-->
                              <th>Text</th><!--new-->
                              <th colspan="3"></th>
                         </tr>
                  </thead>
            <tbody>
                   <% @articles.each do |article| %>
                    <tr>
                               <td><%= article.title %></td><!--new-->
                              <td><%= article.text %></td> <!--new-->
                              <td><%= link_to 'Show', article %></td>
                              <td><%= link_to 'Edit', edit_article_path(article) %></td>
                              <td><%= link_to 'Destroy', article, method: :delete, data: { confirm: 'Are you sure?' } %></td>
                     </tr>
                    <% end %>
                   </tbody>
            </table>
            <br>
            <%= link_to 'New Article', new_article_path %>
            
         3.修改show.html.erb文件为：
         <p id="notice"><%= notice %></p>
            <p>
             <strong>Title:</strong>
            <%= @article.title %>
            </p>
            <p>
             <strong>Text:</strong>
             <%= @article.text %>
            </p>
            <%= link_to 'Edit', edit_article_path(@article) %> |
            <%= link_to 'Back', articles_path %>
            
   *修改app/models/article.rb文件，修改为
      
            class Article < ApplicationRecord
                  validates :title, presence: true,
                   length: { minimum: 3 }
            end
            
  ### 这样我们成功建立了一个可以新建、读取查询、更新和删除文章的blog系统
      
      
   
