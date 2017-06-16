---
layout: post
title:  "Rails Portfolio Project: Trainer Client Tracker"
date:   2017-06-16 02:24:10 -0400
---


   For my Rails Project, I created an app inspired by my previous career: an app where personal trainers can keep track of the clients they have, the progress they make towards their personal goals, and the dates and times of upcoming appointments. When I conceptualized the idea for this app, I thought that my previous experience as a personal trainer would make this an easy project to build. 
	 
   Boy, was I wrong. Building a working version of this app has been by far my most difficult and frustrating experience at the Learn Verified Online Program, which one might infer by the fact that I’m writing this blog post about a month after starting this project. My troubles came from a combination of running into several frustrating, (and at sometimes unexplainable) bugs, and also from the fact that I was a little rusty on many concepts from the Rails portion of the curriculum, since it took me a little longer than most to get through it.
	 
   Fortunately, I see this as a great learning experience for myself, as I persevered through many moments (as recently as a few days ago) where I considered scrapping the project altogether and going back to the drawing board to come up with a new idea. The experience also highlighted the mistakes I made, in which the project would have probably gone a lot smoother had I thought certain things through a little more. Although this app isn’t perfect, I’m glad that I finished it and made it work the way I originally intended.

   The first thing I should have thought through more was the data model itself. It originally consisted of 3 models: `Users`, `Appointments`, and `Clients`. `Users` have many `appointments`, and have many `clients` through `appointments`. `Clients` have many `appointments`, and have many `users` through `appointment`s. `Appointments` acts as a join table, belonging to both a `client` and a `user`. 

   In my excitement to build the app, I quickly established the above data model and started working on the app, quickly implementing a devise-less user authentication system. I initially had difficulty implementing an authorization system. I wanted to ensure that users would only be able to see the clients that belonged to them. However, I suddenly realized that with the way my data model was set up, users and clients were not directly associated with each other. This meant that there was no way to tell if a client belonged to a user through code such as `@current_user.clients` without creating an appointment object to explicitly state the association between the user and the client through the appointment’s `:user_id` and `:client_id`. So, to fix this problem, I figured I would somehow set this association through a nested form.
	 
When I started to think about my nested form and what it would look like, I wasn’t sure which path to take. Should my main form have a user create a new client with a nested form for their first appointment with that client? Or should I have users click on a “Schedule an appointment for a new client” link that would take them to a new appointment form with the new client as the nested form? I decided to play it safe and build out both of these options to see how they fared out. I intended to implement both of them into my app if I were able to get both of them to work.
	 
   Things were going well with both of my options until it came to validations. To illustrate the problem I was running into, imagine I was filling out my new client form with a nested attribute for the client’s first appointment. I fill out all the fields for both my client and appointment, and hit submit. Then, I realize I forgot to fill out the client’s weight field and accidentally left it blank, resulting in a validation error for my client. However, even though the client is invalid, the appointment still gets created anyway, without a set `:client_id`. The same thing happened with my new appointment form, with the client being created without an `:appointment_id` when the appointment was invalid.
	 
I was able to fix the problem in the new client form by placing an `if self.valid` statement at the beginning of my `appointments_attributes=` custom attribute writer. Unfortunately, this did not work for my new appointment form and my `client_attributes=` custom writer. After spending a considerable amount of time trying to find a solution to no avail, I decided to scrap the new appointment form and keep my app simple by sticking with the new client form with the nested form for the client’s appointments.
		
Once I had my nested form in place, along with my client and appointment index pages, I began to work on implementing the main feature of my app: I wanted user’s to be able to document their client’s progress based on their goal (whether they want to lose weight or gain weight) over time. I initially planned to implement this feature by having a button for each appointment called “Appointment Complete”, which the user would click on whenever the user completes an appointment with one of their clients. Clicking on this button sends a get request to the `‘clients/:id/appointment_complete’` route, which then executes the `appointment_complete` action in the client’s controller to take the user to a special form for the client model. This form only has one field for the client’s `:weight_change` attribute, which would represent the amount of weight the client gained or lost since the last appointment. If it's the client's first appointment, this `:weight_change` field does not appear. The form also has a nested form for the associated appointment object, where the user will enter a new date for the client’s next appointment.
		
This brings me to one of the more controversial decisions I made regarding this app, specifically the `Appointment` model itself. Since the only attribute of an appointment object other than `:client_id` and `:user_id` is `:date`, I decided to maintain a single appointment object between a user and client. I thought it would be better if the `:date` attribute would simply be edited every time a client completes an appointment. This way, the relationship between the user and client is maintained, and appointment objects are not being constantly created and destroyed. I know it does not follow normal Rails conventions to do this, or for Appointments to be exclusively created through a nested form in the client form, but I felt that doing it this way made the app work in the simplest way possible.

Once the user submits the form, a patch request is sent to the `'clients/:id/update_progress'` route, which executes the `update_progress` action in the client’s controller:

```
def update_progress
  @client = current_user.clients.find(params[:id])
  if @client.valid?
	  if @client.completed_appointments != 0
      @client.document_progress(client_params[:weight_change].to_i)
      @client.update(client_params)
		end
    @client.complete_appointment
    redirect_to client_path(@client)
  else
    render :appointment_complete
  end
end
```

Here, I intended to use a method called `document_progress` to take the value of the `:weight_change` attribute provided by the params hash and use it to either add to or subtract from the client's current weight based on the client's goal, and then save that end result as the client's `:weight_change` attribute. Here was what I originally intended the method to look like: 

```
def document_progress(new_weight)
    if self.goal == "Lose Weight"
      self.update(weight_change: (self.weight - new_weight))
    elsif self.goal == "Gain Weight"
      self.update(weight_change: (new_weight - self.weight))
    end
  end
```

While this wasn't the most simple implementation of what I wanted to occur, it worked... or so I thought. When I tried to test this feature out, I kept running into a `nil can't be coerced into Fixnum (TypeError)` error in the `document_progress` method when it attempted to update the `:weight_change` attribute. When I placed a `binding.pry` right before the method updates the `:weight_change` attribute, I found that `self.weight_change` would return nil, even though when entering `self` would list the client's `:weight_change` as 0 along with the other attributes. 

Even when I added `default: 0` and `null: false` to the migration where I added the `:weight_change` attribute, it would still return nil in the method. My frustration reached a boiling point when I went into the rails console and edited the client's `:weight_change` attribute to a number other than zero. Still, calling `self.weight_change` in the `binding.pry` would return nil. I couldn't get the most important method of my app to work because the main attribute I wanted to update was being reset to nil in the middle of the method for no reason at all. 

After a few days of making no progress on solving the problem, I decided to reach out a student named Yianna, who had previously graduated from the program and who I had gotten to know through a few of her study groups I had attended. She gave me an idea which eventually became the solution to my problem: I created a new model called `WeightHistory`. Each `WeightHistory` object has a `:weight_recording` attribute, and belongs to a client. I replaced the `:weight_change` field in the `appointment_complete` complete form with a `:weight` field, and then removed the `:weight_change` field from my clients table. I then added a new attribute to my client's table: `:progress`.

Here is my final `update_progress` action:

```
def update_progress
  @current_weight = @client.weight
  if @client.update(client_params)
    if @client.completed_appointments != 0
      @client.document_progress(client_params[:weight].to_i, @current_weight)
      @client.update_progress
    end
    @client.complete_appointment
    redirect_to client_path(@client)
  else
    render :appointment_complete
  end
end
```

Here, `@current_weight` is used to store the client's old weight so it can be used as an argument to be passed into the `document_progress` method before the client's weight is updated with the new weight. Here is the final `document_progress` method:

```
def document_progress(new_weight, old_weight)
  if self.goal == "Lose Weight"
    self.weight_histories.create(weight_recording: (old_weight - new_weight))
  elsif self.goal == "Gain Weight"
    self.weight_histories.create(weight_recording: (new_weight - old_weight))
  end
end
```

After the `document_progress` method is completed, the `update_progress` method is called:

```
def update_progress
  progress = 0
  self.weight_histories.each do |w|
    progress += w.weight_recording
  end
  self.update(progress: progress)
end
```

Here, the `:weight_recording`'s of each `WeightHistory` object belonging to the client and added up and set equal to a local variable called `progress`. The client's `:progress` attribute is then updated to equal the value of the `progress` variable. Finally, the `complete_appointment` method is called, which simply adds 1 to the client's `completed_appointments` attribute.

After overcoming these problems after much difficulty, I felt like a huge "weight" had been lifted off my back. While I did run into a few more frustrating problems that I won't mention here, I managed to find a solution for mostly all of them. Again, finishing this project came down to perserverance and never giving up in the presence of what it seemed like endless errors. I am very eager to move on from this project as soon as I can and finally move into the JavaScript section of the course. 


