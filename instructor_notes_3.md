## Add a Product category

##### Create a category for each product. 

```
rails g migration AddCategoryToProduct category
rake db:migrate
```

Update the seeds.rb to have these product categories.
(Apparel,Kitchen, Entertainment and Software)


_Note: you may have to reset the DB, this will drop, create, migrate and seed the DB._

```
rake db:reset
```


Update the product_params method in the products controller.  

```
  params.require(:product).permit([:name, :description, :price, :category])
```

Update the product views to have a category

### Add Product Validations

Go to the rails guide for validations. 

* Create a validation for the Product name. Add this to the Product model.  

```
  # A Product MUST have a name
  validates :name, presence: true

```

* Try to create a product without a name
_Notice that we are redirected back to the new product form, WHY?_

* Look at the server log, ALWAYS the go to place for errors!!!!

* Now lets put a binding pry in the create action and try again. 

* After creating the @product call @product.new_record?, @person.valid?. Look this up!!

### ActiveModel::Errors

[Rails Errors](http://guides.rubyonrails.org/active_record_validations.html#working-with-validation-errors)

Add the below to the product new and edit views.  


```
product.errors.messages

 # Displaying Validation Errors in Views. 

  <% if @product.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(@product.errors.count, "error") %> prohibited this
      post from being saved:</h2>
     <ul>
        <% @product.errors.full_messages.each do |msg| %>
         <li><%= msg %></li>
         <% end %>
      </ul>
    </div>
  <% end %>

```

### More Validations

```
  CATEGORIES = ['Apparel', 'Kitchen', 'Entertainment', 'Software']

  # A Product name MUST be at least 3 characters.  
  validates :name, length: { minimum: 3}
  # A Product name MUST be unique
  validates :name, uniqueness: true
  # The price MUST be a number
  validates :price, numericality: true
  validates :price, format: { :with => /^\d+(\.\d{2})?$/ }

  # A category must be one the above
  validates :category, inclusion: { in: CATEGORIES }
```

Validations that must match a regular expression
[ See Regular Expresssion](http://www.regular-expressions.info/)

```
  # The price MUST be of the format that 'matches' the regular expression, regex.
  validates :price, format: { :with => /\A\d+(\.\d{2})?\z/, message: 'Must be a zero or more digits, a period, followed by two digits' }

```

## LAB 

Create a Song that:  
* Must have a unique title and must be between 3 and 15 characters long.  
* May have a genre. If it does have a genre it must be Rock, Pop, Blues, Rap, or Russian Chanson.   
* Unique ID, need to add a uuid integer attribute to the Song model. Find a good regex  
* for this unique id.


