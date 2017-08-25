
Welcome to Intereview Questions!
===================


### __What have you learned recently about iOS development? How did you learn it? Has it changed your approach to building apps?__

----

 - I have recently mastered the use of many advanced features of the Swift programming language.  The understanding and support of a recent language like Swift will prove to be helpful as Swift comes with a lot of handy features like extensions, delegates, protocols, typealieses, etc. These let you focus on building more generic code, which you can make use of in several applications. I started using a great dependency manager called Cocoapods which will allow me to cluster my components into reusable modules that I can include into future projects saving a lot of time and making myself more efficient.
 - Deciding to take up the iOS Swift nanodegree program helped me not just in bringing my knowledge up to speed to the latest technology but also helped me implement and apply best practices to my work. It taught me the importance of documenting my work and how to best use Git for all my developments. I plan to use Git to its full advantage by writing incredibly detailed commit messages, committing often, resolving conflicts, etc. All of this will not only help me create better software, but also benefits the people I work with

### __Can you talk about a framework that you've used recently (Apple or third-party)? What did you like/dislike about the framework?__

----

I recently had a requirement in which I was supposed to integrate a third party authentication system to give access to the resources of my application. I ended up using Google’s Firebase to give my application users access to personalised access to my application. I was very impressed with the document and hep videos that were created for the integration. Not forgetting the easy implementation not just with the UI needed for its integration but also the sustainability of the product. Implementing this feature I even got exposure to the POD concept which made me think of creating one for myself. Something that would take care of all the api calls needed day in and out by any application.


### __Describe how you would construct a Twitter feed application (here is an example of Udacity's Twitter feed) that at minimum can display a company's Twitter page. Please include information about any classes/structs that you would use in the app. Which classes/structs would be the model(s), the controller(s), and the view(s)?__

----

The way I would approach the problem would be to break it down into requirements and specifications as detailed as possible. Once the specifications are mapped out, I would structure the application's data structs and classes following the *Model View Controller* paradigm. For this example I would take the following approach.

 -  ### Model
 	- The Model would be responsible for any incoming traffic from the Twitter API. Things like Account information, Tweets etc. while also taking care of interacting with CoreData. `CoreDataManager` class which will take care of interacting with CoreData and also an `ImageHelper` class for caching and downloading all relevent images from Twitter API.

		| Class                 |  Inherits From |
		|-----------------------|----------------|
		| CoreDataManager  		| N/A            |
		| TwitterAPI            | NSObject       |
		| ImageHelper           | NSObject       |
		| Tweet                 | NSManagedObject|
		| Account               | NSMangedObject |
		| Organization          | NSManageObject |


 -  ### View
 	- For custom views, there is a custom tableview cell for showing the detail of each tweet in the feed.  To customize the UI, there is a *Star* button for marking a tweet, retweet button, forward tweet button, etc.

		|  Class       		 | Inherits From  |
		|--------------------|----------------|
		| StarButton   		 | UIButton       |
		| ReTweetButton   	 | UIButton       |
		| ForwardTweetButton | UIButton       |
		| TweetTableViewCell | UITableViewCell |
        
 -  ### Controller
	- The controller classes are responsible for controlling the flow of the UI. There should be a Login screen, Account screen, Tweet Screen all embeded into a Navigation Controller for easy navigation. 

		| Class                      | Inherits From         |
		|----------------------------|-----------------------|
		| AccountViewController      | UIViewController      |
		| TweetViewController        | UIViewController      |
		| AccountViewController      | UIViewController      |
		| TweetViewController        | UIViewController      | 
		| FeedViewController         | UITableViewController |
		| OrganizationViewController | UIViewController      |
		| NavigationController       | UINavigationController|
		| LoginViewController        | UIViewController      |
		| ComposeTweetViewController | UIViewController      |
        
The above list may not be complete, but it outlines a great start to building a Twitter applicaiton.


### __Describe some techniques that can be used to ensure that a UITableView containing many UITableViewCell is displayed at 60 frames per second.__

----

 - All the work that the apple developers have put into allocating and deallocating the table view cell in the background is remarkable. But with such specific requirments it would come upon the developer to best make use of the tools provided.
 - In general case the automatic allocation and deallocation of the cells is effecient enough. There comes some limitation to assigning the entire tableview with the outlets like delegate and datasource.
 - We can further finetune the allocation process with the help of the following method 
 ``` 
 func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
        code
    } 
```
   - This function give you command over the action that needs to be done when a new cell is about to be allocated.
