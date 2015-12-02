

## Making your code object oriented

We've learned how to do basic scraping, but remember that in ruby it's best practice to make things object oriented, so that our programs can be scalable (they can grow) and mutable (easy to change). Thus, let's work on making our scraping code object oriented. Here is our current code:

```ruby
require 'open-uri'
require 'nokogiri'

fidi_html = open("https://s3-us-west-2.amazonaws.com/nokogiri-scrape/index.html")

fidi_nokogiri = Nokogiri::HTML(fidi_html)

title = fidi_nokogiri.css("h1").text

sophies = fidi_nokogiri.css("div.restaurant.r3 h3").text

restaurants_names = fidi_nokogiri.css("div.restaurant h2").collect do | name|
  name.text
end

```

Let's first wrap all of our code (except for requiring gems) inside a class:

```ruby
require 'open-uri'
require 'nokogiri'

class ScrapeRestaurants

  fidi_html = open("https://s3-us-west-2.amazonaws.com/nokogiri-scrape/index.html")

  fidi_nokogiri = Nokogiri::HTML(fidi_html)

  title = fidi_nokogiri.css("h1").text

  sophies = fidi_nokogiri.css("div.restaurant.r3 h3").text

  restaurants_names = fidi_nokogiri.css("div.restaurant h2").collect do | name|
    name.text
  end

end

```

Now we need to initialize with the data we want in the object:

```ruby
  def initialize
    @fidi_html = open("https://s3-us-west-2.amazonaws.com/nokogiri-scrape/index.html")
    @fidi_nokogiri = Nokogiri::HTML(fidi_html)
  end
```
We turn our fidi_html and fidi_nokogiri variables into instance variables by using the `@` sign. We do this so that the data in these variables can be read by other methods in the class

Next, we create methods for the actions we want our object to have (the things we want it to do!): `get_page_title`, `get_sophies_name`, `list_restaurants_names`, etc. Wrap the code for each of these inside of a method. 

```ruby
require 'open-uri'
require 'nokogiri'

class ScrapeRestaurants

  def initialize
    @fidi_html = open("https://s3-us-west-2.amazonaws.com/nokogiri-scrape/index.html")
    @fidi_nokogiri = Nokogiri::HTML(@fidi_html)
  end

  def get_page_title
    title = @fidi_nokogiri.css("h1").text
  end

  def get_sophies_name
    sophies = @fidi_nokogiri.css("div.restaurant.r3 h2").text
  end

  def list_restaurants_names
    restaurants_names = @fidi_nokogiri.css("div.restaurant h2").collect do |name|
      name.text
    end
  end

end
```

Then we can call these methods on an instance of the class:

```ruby
fidi_scrape = Scrape.new
puts fidi_scrape.get_page_title
puts fidi_scrape.get_sophies_name
puts fidi_scrape.list_restaurants_names

```
That's all there is to it!



<a href='https://learn.co/lessons/hs-oo-scraper-walkthrough' data-visibility='hidden'>View this lesson on Learn.co</a>
