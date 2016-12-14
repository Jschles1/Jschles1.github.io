---
layout: post
title:  "My CLI Data Gem: Best Selling Games"
date:   2016-12-13 22:50:33 -0500
---


Today I am putting the final touches on my CLI Data Gem. I am extremely satisfied with how it turned out, even though it was sort of a long process figuring it all out. Completing this gem has definitely been my most satisfying experience at the Flatiron School thus far.

The hardest part of this project was actually coming up with an idea for what to do. After a few days, I decided to do my project based on one of my main interests since childhood, video games. What resulted was the Best Selling Games gem.

This gem lists the current best selling games of the video game system you choose. You can then select which listed game you want more details on, including the game's publisher, price, and a link to the game's webpage for more information. Systems that can be chosen include Xbox One, Playstation 4, Wii-U, Nintendo 3DS, and PC. All of the data is scraped from GameStop's website, www.gamestop.com.

I feel that this gem can be especially useful during this time of year, where many people are buying video games for loved ones as gifts for the holidays. A common problem with buying video games as gifts is that sometimes the person buying the game isn't really a gamer themself, so they aren't really familiar with which games are popular right now. This gem helps greatly with this problem.

When it came to building the program itself, Avi's video walkthrough on his Daily-Deal gem was invaluable. I followed his lead in trying to build parts of the CLI controller first. After that, I created the scraping method for xbox one games and tested it until it worked. From then on, the rest of the program became very easy to write.

The basic flow of the program is as follows:

1. Run the gem and you will be asked to input which system's bestsellers you want to list.
2. After the bestsellers are listed, you will be asked to input which game you want more details on.
3. After the chosen game's details are listed, you will be asked whether you want get details from another game on the list, choose another system's bestsellers to list, or exit the program.

I used two classes in this project, the CLI class and the Game class.

`BestsellingGames::CLI` is responsible for the flow of the program. When the gem is run, the `call` method is called.

```
def call
  welcome
  list_bestsellers
end
```

`welcome` welcomes the user to the app and asks the user which system's bestsellers it wants to list.

```
def welcome
  puts "Welcome to the Best Selling Games App!"
  sleep 1
  print "Which system's current bestselling games would you like to see? (Xbox One, PS4, Wii U, 3DS, PC): "
end
```

Next, `list_bestsellers` is called. This gets the input from the user, and lists the titles of the bestsellers of the system the user chose. If the input is not one of the options shown, an error message is shown and the user has a chance to put in another input. 

```
def list_bestsellers
  input = nil
  input = gets.strip.downcase

  if input == "xbox one"
    @bestsellers = BestsellingGames::Game.xbox_scrape
    puts "Current Xbox One Bestsellers:"
    @bestsellers.each.with_index(1) do |game, i|
      puts "#{i}. #{game.title}"
    end
		get_details
  elsif input == "ps4"
    @bestsellers = BestsellingGames::Game.ps4_scrape
    puts "Current Playstation 4 Bestsellers:"
    @bestsellers.each.with_index(1) do |game, i|
      puts "#{i}. #{game.title}"
    end
		get_details
  elsif input == "wii u"
    @bestsellers = BestsellingGames::Game.wiiu_scrape
    puts "Current Wii U Bestsellers:"
    @bestsellers.each.with_index(1) do |game, i|
      puts "#{i}. #{game.title}"
    end
		get_details
  elsif input == "3ds"
    @bestsellers = BestsellingGames::Game.ds_scrape
    puts "Current Nintendo 3DS Bestsellers:"
    @bestsellers.each.with_index(1) do |game, i|
      puts "#{i}. #{game.title}"
    end
		get_details
  elsif input == "pc"
    @bestsellers = BestsellingGames::Game.pc_scrape
    puts "Current PC Bestsellers:"
    @bestsellers.each.with_index(1) do |game, i|
      puts "#{i}. #{game.title}"
    end
		get_details
  else
    print "Invalid input, please try again: "
    list_bestsellers
  end

end
```

As you can see in this method, new objects from the `BestsellingGames::Game` class are created through the appropriate scrape method. Here is part of the `BestsellingGames::Game` class with the `xbox_scrape` method so you can see what is going on behind the scenes.

```
class BestsellingGames::Game
  attr_accessor :title, :price, :publisher, :url

  def self.xbox_scrape
    return_array = []
    doc = Nokogiri::HTML(open("http://www.gamestop.com/browse/games/xbox-one?nav=28-xu0,131e0-ffff2418"))
    games = doc.css("div.product.new_product")
    games.each_with_index do |game, i|
      game = self.new
      game.title = games.css(".ats-product-title-lnk")[i].text.gsub("- Only at GameStop", "").strip

      price_scrape = games.css("p.pricing.ats-product-price")[i].text.strip
      if price_scrape.length > 7
        game.price = price_scrape.gsub("$", " ").split(" ")[1].insert(0, "$")
      else
        game.price = price_scrape
      end

      game.publisher = games.css(".publisher.ats-product-publisher")[i].text.gsub("by ", "").strip
      game.url = games.css(".ats-product-title-lnk")[i].attr("href").split.insert(0, "www.gamestop.com").join
      return_array << game
    end

    return_array

  end
end
```

The attributes of each `Game` object are set equal to the appropriate piece of scraped data. Each `Game` object is then stored into an array for later use. Each system has its own scraping method in the `BestsellingGames::Game` class.

I had initially tried to include an attribute for a games rating (or score), but I encountered an unforeseen problem that I could not find the solution to and had to scrap it. The problem was that not every bestseller had a rating, as some of them had just been released and did not receive enough ratings from customers to get an average rating. This caused problems with the assigning of attributes based on the scraped data. Ratings from certain games would get assigned to others, and certain games that HAD ratings would not be assigned a rating at all, or would be assigned a rating that actually belonged to another game. Perhaps I'll find a solution to this problem in the future and include it in an update.

Now, back to `BestsellingGames::CLI`. After `list_bestsellers` outputs the list of bestselling titles for the system chosen, `get_details` is called. This asks the user to choose which game on the list they want more details from, then lists the details of the chosen game.

```
def get_details
  input = nil
  puts ""
  print "Please enter the number of the game you want more details on: "
  input = gets.strip.downcase

  if input.to_i > 0 && input.to_i < 13
    game = @bestsellers[input.to_i-1]
    puts "*******************************************************************"
    puts "#{game.title}:"
    puts "Published by: #{game.publisher}"
    puts "Current price: #{game.price}"
    puts "For more info, go to: #{game.url}"
    puts "*******************************************************************"
  else
    puts "Invalid input, please try again."
    get_details
  end
	
	decision
	
end
```

After the the details of the game are listed, `decision` is called. This gives the user a few options on what to do next. They can either get details from another game on the list, get a list of another system's bestsellers, or they can choose to exit the program. The appropriate method is then called based on the input.

```
def decision
  puts "Type (1) if you would like to get details on another game from the list."
  puts "Type (2) if you would like to choose another system's bestsellers to list."
  puts "Type (3) if you would like to exit the program."

  input = nil
  input = gets.strip

  if input.to_i == 1
    get_details
  elsif input.to_i == 2
    print "Which system's current bestselling games would you like to see? (Xbox One, PS4, Wii U, 3DS, PC): "
    list_bestsellers
  elsif input.to_i == 3
    close_program
  else
    puts "Invalid input, please try again."
    decision
  end

end
```

After struggling greatly with the previous projects and doubting my ability to complete this course, I can happily say that completing this project all on my own has given me great confidence in my future as a potential full stack web developer. I now look forward to meeting the challenges ahead.

Github Repo Link: https://github.com/Jschles1/bestselling_games-cli-app




