!SLIDE
#Formtastic#
##Lose the Zero and Get with the Hero##

<div class="personal-info">
<ul>
<li>Keith Gaddis</li>
<li>Inductive Applications
<li>@karmajunkie</li>
<li>keith.gaddis@advanceclaim.com</li>
</ul>
</div>



!SLIDE smbullets incremental
##Every n00b's favorite Rails feature: scaffolding##
###Problems with scaffolds
* static, don't grow with models
* not Smart™

!SLIDE code smaller
##Scaffold example
    <%= form_for(@blog) do |f| %>
      <% if @blog.errors.any? %>
        <div id="error_explanation">
        <h2><%= pluralize(@blog.errors.count, "error") %> 
          prohibited this blog from being saved:</h2>

          <ul>
          <% @blog.errors.full_messages.each do |msg| %>
              <li><%= msg %></li>
          <% end %>
          </ul>
        </div>
      <% end %>
      <div class="field">
          <%= f.label :title %><br />
          <%= f.text_field :title %>
      </div>
      <div class="field">
          <%= f.label :body %><br />
          <%= f.text_area :body %>
      </div>
      <div class="actions">
          <%= f.submit %>
      </div>
    <% end %>

!SLIDE bullets incremental
#Formtastic: the better way
* dynamic
* intelligent fields
* handles associations
* semantic output
* (still just FormBuilder)

!SLIDE code smaller
#Some data
    # Table name: categories
    #
    #  id         :integer         not null, primary key
    #  name       :string(255)

    class Category < ActiveRecord::Base
      has_and_belongs_to_many :blogs
    end

    # Table name: authors
    #
    #  id         :integer         not null, primary key
    #  name       :string(255)
    #  email      :string(255)

    class Author < ActiveRecord::Base 
      has_many :blogs
    end

!SLIDE code smaller
#And still more data...
    # == Schema Information
    #
    # Table name: blogs
    #
    #  id         :integer         not null, primary key
    #  body       :text
    #  title      :string(255)
    #  author_id  :integer

    class Blog < ActiveRecord::Base
      belongs_to :author
      has_and_belongs_to_many :categories
    end

!SLIDE code smaller
#Scaffolding
##(with a side of Formtastic)
    <%= semantic_form_for blog do |f| %>
      <%= f.inputs %>
      <%= f.buttons %>
    <% end %>


!SLIDE form
#Scaffolding
 
<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> 
  <fieldset class="inputs"><ol><li class="select optional" id="blog_author_input"><label for="blog_author_id">Author</label><select id="blog_author_id" name="blog[author_id]"><option value=""></option> 
<option value="1" selected='true'>karmajunkie</option></select></li><li class="text optional" id="blog_body_input"><label for="blog_body">Body</label><textarea id="blog_body" name="blog[body]" rows="3"></textarea></li><li class="string optional" id="blog_title_input"><label for="blog_title">Title</label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li></ol></fieldset> 
  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> 
</form>

!SLIDE code smaller
# Beyond scaffolds

    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Entry details", :title, :body %>
      <%= f.buttons %>
    <% end %>


!SLIDE form
#Beyond scaffolds

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> 
  <fieldset class="inputs"><legend><span>Entry details</span></legend><ol><li class="string optional" id="blog_title_input"><label for="blog_title">Title</label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li><li class="text optional" id="blog_body_input"><label for="blog_body">Body</label><textarea id="blog_body" name="blog[body]" rows="3"></textarea></li></ol></fieldset> 
  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> 
</form> 

!SLIDE code smaller
#Being specific

    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Entry details" do %>
        <%= f.input :author %>
        <%= f.input :title %>
        <%= f.input :body %>
      <% end %>
      <%= f.buttons %>
    <% end %>

!SLIDE form
#Being specific
<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="select optional" id="blog_author_input"><label for="blog_author_id">Author</label><select id="blog_author_id" name="blog[author_id]"><option value=""></option> <option value="1">karmajunkie</option> <option value="2">thoreau</option> <option value="3">shakespeare</option></select></li> <li class="string optional" id="blog_title_input"><label for="blog_title">Title</label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="text optional" id="blog_body_input"><label for="blog_body">Body</label><textarea id="blog_body" name="blog[body]" rows="3"></textarea></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> </form> 

!SLIDE code small
#Changing defaults

    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Entry details" do %>
        <%= f.input :author, :as => :radio %>
        <%= f.input :title %>
        <%= f.input :body, :as => :string %>
      <% end %>
      <%= f.buttons %>
    <% end %>

!SLIDE form
#Results
<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="radio optional" id="blog_author_input"><fieldset><legend class="label"><label>Author</label></legend><ol><li><label for="blog_author_id_1"><input id="blog_author_id_1" name="blog[author_id]" type="radio" value="1" /> karmajunkie</label></li><li><label for="blog_author_id_2"><input id="blog_author_id_2" name="blog[author_id]" type="radio" value="2" /> thoreau</label></li><li><label for="blog_author_id_3"><input id="blog_author_id_3" name="blog[author_id]" type="radio" value="3" /> shakespeare</label></li></ol></fieldset></li> <li class="string optional" id="blog_title_input"><label for="blog_title">Title</label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="string optional" id="blog_body_input"><label for="blog_body">Body</label><input id="blog_body" name="blog[body]" type="text" /></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> </form> 
 
!SLIDE smbullets incremental
#Additional options
* <code>:hint</code> — adds text below input
* <code>:required</code> — signal required fields
* <code>:label</code> — change the default label text (very versatile)
* <code>:input_html</code> — options for the input html attributes
* <code>:button_html</code> — html options for the form buttons
* <code>:wrapper_html</code> — html options for the &lt;li&gt; wrapper elements

!SLIDE code small
#Additional options example

    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Entry details" do %>
        <%= f.input :author, 
          :as => :radio, 
          :hint => "Who wrote this?" %>
        <%= f.input :title, :required => true %>
        <%= f.input :body, 
          :as => :string, 
          :label => "Post text" %>
      <% end %>
      <%= f.buttons %>
    <% end %>

!SLIDE form
#Additional options example

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="radio optional" id="blog_author_input"><fieldset><legend class="label"><label>Author</label></legend><ol><li><label for="blog_author_id_1"><input id="blog_author_id_1" name="blog[author_id]" type="radio" value="1" /> karmajunkie</label></li><li><label for="blog_author_id_2"><input id="blog_author_id_2" name="blog[author_id]" type="radio" value="2" /> thoreau</label></li><li><label for="blog_author_id_3"><input id="blog_author_id_3" name="blog[author_id]" type="radio" value="3" /> shakespeare</label></li></ol></fieldset> <p class="inline-hints">Who wrote this?</p></li> <li class="string required" id="blog_title_input"><label for="blog_title">Title<abbr title="required">\*</abbr></label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="string optional" id="blog_body_input"><label for="blog_body">Post text</label><input id="blog_body" name="blog[body]" type="text" /></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> </form> 
 
!SLIDE code small
#HABTM associations
    class Blog < ActiveRecord::Base
      belongs_to :author
      has_and_belongs_to_many :categories
    end

    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Entry details" do %>
        <%= f.input :author, :as => :radio %>
        <%= f.input :categories, :as => :check_boxes %>
        <%= f.input :title, :required => true %>
        <%= f.input :body, :as => :string %>
      <% end %>
      <%= f.buttons %>
    <% end %>
    
!SLIDE form
#HABTM associations 

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="radio optional" id="blog_author_input"><fieldset><legend class="label"><label>Author</label></legend><ol><li><label for="blog_author_id_1"><input id="blog_author_id_1" name="blog[author_id]" type="radio" value="1" /> karmajunkie</label></li><li><label for="blog_author_id_2"><input id="blog_author_id_2" name="blog[author_id]" type="radio" value="2" /> thoreau</label></li><li><label for="blog_author_id_3"><input id="blog_author_id_3" name="blog[author_id]" type="radio" value="3" /> shakespeare</label></li></ol></fieldset></li> <li class="check_boxes optional" id="blog_categories_input"><fieldset><legend class="label"><label>Categories</label></legend><input id="blog_category_ids_" name="blog[category_ids][]" type="hidden" value="" /><ol><li><label for="blog_category_ids_1"><input id="blog_category_ids_1" name="blog[category_ids][]" type="checkbox" value="1" /> poetry</label></li><li><label for="blog_category_ids_2"><input id="blog_category_ids_2" name="blog[category_ids][]" type="checkbox" value="2" /> prose</label></li><li><label for="blog_category_ids_3"><input id="blog_category_ids_3" name="blog[category_ids][]" type="checkbox" value="3" /> drivel</label></li></ol></fieldset></li> <li class="string required" id="blog_title_input"><label for="blog_title">Title<abbr title="required">\*</abbr></label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="string optional" id="blog_body_input"><label for="blog_body">Body</label><input id="blog_body" name="blog[body]" type="text" /></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> </form> 
 
!SLIDE code  small
#HABTM as selects

    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Entry details" do %>
        <%= f.input :author, :as => :radio %>
        <%= f.input :categories, :as => :select%>
        <%= f.input :title, :required => true %>
        <%= f.input :body, :as => :string %>
      <% end %>
      <%= f.buttons %>
    <% end %>

!SLIDE form
#HABTM as selects

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="radio optional" id="blog_author_input"><fieldset><legend class="label"><label>Author</label></legend><ol><li><label for="blog_author_id_1"><input id="blog_author_id_1" name="blog[author_id]" type="radio" value="1" /> karmajunkie</label></li><li><label for="blog_author_id_2"><input id="blog_author_id_2" name="blog[author_id]" type="radio" value="2" /> thoreau</label></li><li><label for="blog_author_id_3"><input id="blog_author_id_3" name="blog[author_id]" type="radio" value="3" /> shakespeare</label></li></ol></fieldset></li> <li class="select optional" id="blog_categories_input"><label for="blog_category_ids">Categories</label><select id="blog_category_ids" multiple="multiple" name="blog[category_ids][]" size="5"><option value="1">poetry</option> <option value="2">prose</option> <option value="3">drivel</option></select></li> <li class="string required" id="blog_title_input"><label for="blog_title">Title<abbr title="required">\*</abbr></label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="string optional" id="blog_body_input"><label for="blog_body">Body</label><input id="blog_body" name="blog[body]" type="text" /></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset></form> 

!SLIDE code small
#Limiting the collection
    class Author < ActiveRecord::Base
      has_many :blogs
      scope :famous, where(:famous => true)
    end

    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Entry details" do %>
        <%= f.input :author, 
              :as => :radio, 
              :collection => Author.famous %>
        <%= f.input :categories, :as => :select%>
        <%= f.input :title, :required => true %>
        <%= f.input :body, :as => :string %>
      <% end %>
      <%= f.buttons %>
    <% end %>

!SLIDE form
#Limiting the collection

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="radio optional" id="blog_author_input"><fieldset><legend class="label"><label>Author</label></legend><ol><li><label for="blog_author_id_2"><input id="blog_author_id_2" name="blog[author_id]" type="radio" value="2" /> thoreau</label></li><li><label for="blog_author_id_3"><input id="blog_author_id_3" name="blog[author_id]" type="radio" value="3" /> shakespeare</label></li></ol></fieldset></li> <li class="select optional" id="blog_categories_input"><label for="blog_category_ids">Categories</label><select id="blog_category_ids" multiple="multiple" name="blog[category_ids][]" size="5"><option value="1">poetry</option> <option value="2">prose</option> <option value="3">drivel</option></select></li> <li class="string required" id="blog_title_input"><label for="blog_title">Title<abbr title="required">\*</abbr></label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="string optional" id="blog_body_input"><label for="blog_body">Body</label><input id="blog_body" name="blog[body]" type="text" /></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset></form> 
 
!SLIDE bullets incremental
#HTML5 input types
* <code>:email</code>
* <code>:url</code>
* <code>:search</code>

!SLIDE smbullets incremental
#Additional input types
* <code>:time_zone</code>
* <code>:password</code>
* <code>:date/:datetime/:time</code>
* <code>:number</code>
* <code>:file</code>
* <code>:country</code>
* <code>:hidden</code>
* easy to add new input types

!SLIDE code smaller
#Nested resources
    <%= semantic_form_for blog do |f| %>
      <%= f.inputs "Author details", :name, :email, 
         :for => :author %>
      <%= f.inputs "Entry details" do %>
        <%= f.input :title, :required => true %>
        <%= f.input :body, :as => :string %>
      <% end %>
      <%= f.buttons %>
    <% end %>

!SLIDE form
#Nested resources

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Author details</span></legend><ol><li class="string required" id="blog_author_name_input"><label for="blog_author_name">Name<abbr title="required">\*</abbr></label><input id="blog_author_name" name="blog[author][name]" type="text" /></li><li class="string required" id="blog_author_email_input"><label for="blog_author_email">Email<abbr title="required">\*</abbr></label><input id="blog_author_email" name="blog[author][email]" type="text" /></li></ol></fieldset> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="string required" id="blog_title_input"><label for="blog_title">Title<abbr title="required">\*</abbr></label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="string optional" id="blog_body_input"><label for="blog_body">Body</label><input id="blog_body" name="blog[body]" type="text" /></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> </form> 

!SLIDE smbullets incremental
#Styling
* formtastic stylesheets provided by generator (formatastic:install)
* [make changes in public/stylesheets/formtastic_changes.css]
* Sass stylesheets also available (https://github.com/reidab/formtastic-sass)

!SLIDE smbullets
#Internationalization
* Labels can use ActiveRecord i18n, or Formtastic's own implementation

<pre><code>
en:
  activerecord:
    attributes:
      blog: 
        title: "Name of post"
</code></pre>

!SLIDE form
#I18n, ActiveRecord

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="select optional" id="blog_author_input"><label for="blog_author_id">Author</label><select id="blog_author_id" name="blog[author_id]"><option value=""></option> <option value="1">karmajunkie</option> <option value="2">thoreau</option> <option value="3">shakespeare</option></select></li> <li class="string required" id="blog_title_input"><label for="blog_title">Name of post<abbr title="required">\*</abbr></label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="text optional" id="blog_body_input"><label for="blog_body">Body</label><textarea id="blog_body" name="blog[body]" rows="4"></textarea></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> </form> 

!SLIDE code smaller
#Formtastic i18n

    #config/initializers/formtastic.rb
    Formtastic::SemanticFormBuilder.i18n_lookups_by_default = true

    #config/locales/en.yml
    en:
      formtastic:
        titles:
          entry: "Entry details"
        labels:
          blog:
            title: "New blog title"
            body: "What's on your mind?"
        hints:
          blog:
            body: "Write something inspiring here."
        placeholders:
          blog:
            title: "Title your post"
        actions:
          create: "Create my %{model}"

!SLIDE form 
#Formtastic i18n
      <%= semantic_form_for blog do |f| %>
        <%= f.inputs :entry do %>
          <%= f.input :author %>
          <%= f.input :title %>
          <%= f.input :body %>
        <% end %>
        <%= f.buttons %>
      <% end %>

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="select optional" id="blog_author_input"><label for="blog_author_id">Author</label><select id="blog_author_id" name="blog[author_id]"><option value=""></option> <option value="1">karmajunkie</option> <option value="2">thoreau</option> <option value="3">shakespeare</option></select></li> <li class="string optional" id="blog_title_input"><label for="blog_title">New blog title</label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="text optional" id="blog_body_input"><label for="blog_body">What's on your mind?</label><textarea id="blog_body" name="blog[body]" rows="4"></textarea> <p class="inline-hints">Write something inspiring here.</p></li> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create my Blog" /></li></ol></fieldset> </form> 
 
!SLIDE bullets
#Bonus material
* validation\_reflection gem (https://github.com/redinger/validation\_reflection)
* country\_select plugin (https://github.com/chrislerum/country\_select)

!SLIDE form
    class Blog < ActiveRecord::Base
      belongs_to :author
      has_and_belongs_to_many :categories

      validates_presence_of :body
      validates_presence_of :title
    end

<form accept-charset="UTF-8" action="/blogs" class="formtastic blog" id="new_blog" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="hVlGug1iWvv2sw0vp5yP7rRrRJ7Xmwl+ITcjDROinXY=" /></div> <fieldset class="inputs"><legend><span>Entry details</span></legend><ol> <li class="select optional" id="blog_author_input"><label for="blog_author_id">Author</label><select id="blog_author_id" name="blog[author_id]"><option value=""></option> <option value="1">karmajunkie</option> <option value="2">thoreau</option> <option value="3">shakespeare</option></select></li> <li class="string required" id="blog_title_input"><label for="blog_title">Title<abbr title="required">\*</abbr></label><input id="blog_title" maxlength="255" name="blog[title]" type="text" /></li> <li class="text required" id="blog_body_input"><label for="blog_body">Body<abbr title="required">\*</abbr></label><textarea id="blog_body" name="blog[body]" rows="3"></textarea></li> <li class="country optional" id="blog_country_input"><label for="blog_country">Country</label><select id="blog_country" name="blog[country]"><option value="Australia">Australia</option> <option value="Canada">Canada</option> <option value="United Kingdom">United Kingdom</option> <option value="United States">United States</option><option value="" disabled="disabled">-------------</option> <option value="Afghanistan">Afghanistan</option> <option value="Aland Islands">Aland Islands</option> <option value="Albania">Albania</option> <option value="Algeria">Algeria</option> <option value="American Samoa">American Samoa</option> <option value="Andorra">Andorra</option> <option value="Angola">Angola</option> <option value="Anguilla">Anguilla</option> <option value="Antarctica">Antarctica</option> <option value="Antigua And Barbuda">Antigua And Barbuda</option> <option value="Argentina">Argentina</option> <option value="Armenia">Armenia</option> <option value="Aruba">Aruba</option> <option value="Australia">Australia</option> </ol></fieldset>  <fieldset class="buttons"><ol><li class="commit"><input class="create" id="blog_submit" name="commit" type="submit" value="Create Blog" /></li></ol></fieldset> </form> 
 
!SLIDE  bullets
#ALL NEW!!!
#MORE BONUSY BONUSES!!!
* Custom inputs 
* (Requires Formtastic 2)
* Create new inputs to support custom data types
* (Totally stolen from documentation at https://github.com/justinfrench/formtastic)

!SLIDE code smaller
##Changing the behavior of an existing input type

    #app/inputs/string_input.rb
    class StringInput < Formtastic::Inputs::StringInput
        def to_html
            puts "this is my modified version of StringInput"
            super
        end
    end

!SLIDE code smaller
##New input type based on old

    # f.input(:body, :as => :flexible_text)
    class FlexibleTextInput < Formtastic::Inputs::StringInput
        def input_html_options
          super.merge(:class => "flexible-text-area")
        end
    end

!SLIDE code smaller
#ALL NEW INPUT!
     # f.input(:created_at, :as => :date_picker)
      class DatePickerInput
        include Formtastic::Inputs::Base
        def to_html
          # ...
        end
      end

