
Welcome to Intereview Questions! 
===================


### __What have you learned recently about iOS development? How did you learn it? Has it changed your approach to building apps?__

----

 - I have recently mastered the use of many advanced features of the Swift programming language.  The understanding and support of a recent language like Swift will prove to be helpful as Swift comes with a lot of handy features like extensions, delegates, protocols, typealieses, etc. These let you focus on building more generic code, which you can make use of in several applications. I started using a great dependency manager called Cocoapods which will allow me to cluster my components into reusable modules that I can include into future projects saving a lot of time and making myself more efficient.
 - Deciding to take up the iOS Swift nanodegree program helped me not just in bringing my knowledge up to speed with the latest technology but also helped me implement and apply best practices to my work. It taught me the importance of documenting my work and how to best use Git for all my developments. I plan to use Git to its full advantage by writing incredibly detailed commit messages, committing often, resolving conflicts, etc. All of this will not only help me create better software but also benefits the people I work with

### __Can you talk about a framework that you've used recently (Apple or third-party)? What did you like/dislike about the framework?__

----

I recently had a requirement in which I was supposed to integrate a third party authentication system to give access to the resources of my application. I ended up using Google’s Firebase to give my application users access to personalized access to my application. I was very impressed with the document and help videos that were created for the integration. Not forgetting the easy implementation not just with the UI needed for its integration but also the sustainability of the product. Implementing this feature I even got exposure to the POD concept which made me think of creating one for myself. Something that would take care of all the API calls needed day in and out by any application.


### __Describe how you would construct a Twitter feed application (here is an example of Udacity's Twitter feed) that at minimum can display a company's Twitter page. Please include information about any classes/structs that you would use in the app. Which classes/structs would be the model(s), the controller(s), and the view(s)?__

----

The way I would approach the problem would be to break it down into requirements and specifications as detailed as possible. Once the specifications are mapped out, I would structure the application's data structs and classes following the *Model View Controller* paradigm. For this example, I would take the following approach.

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
        
The above list may not be complete, but it outlines a great start to building a Twitter application.



### __Describe some techniques that can be used to ensure that a UITableView containing many UITableViewCell is displayed at 60 frames per second.__

----

 - All the work that the apple developers have put into allocating and deallocating the table view cell in the background is remarkable. But with such specific requiremnts, it would come upon the developer to best make use of the tools provided.
 - In general case, the automatic allocation and deallocation of the cells are efficient enough. There comes some limitation to assigning the entire tableview with the outlets like delegate and datasource.
 - We can further fine-tune the allocation process with the help of the following method 
 ``` 
 func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
        code
    } 
```
   - This function gives you command over the action that needs to be done when a new cell is about to be allocated. 



### __Imagine that you have been given a project that has this ActorViewController. The ActorViewController should be used to display information about an actor. However, to send information to other ViewControllers, it uses NSUserDefaults. Does this make sense to you? How would you send information from one ViewController to another one?__

----

The approach suggested in the question does not make sense to me for the following reasons.
 - NSUserDefaults is not a smart solution for persisting any data other than user settings as there are much better solutions for persisting data.
 - My suggestion would be to refactor the code to make use of CoreData also to follow the *Model View Controller* paradigm.  
 - I would recommend creating an individual NSManagedObject model class for the *Actor*.
 - The class would be accessible through the `fetch request` from within any ViewController and simply save changes using `managedObjectContexts` created using a  `CoreDataStackManager` class.

-  Actor CoreDataProperties 

 ```

import CoreData
import Foundation

extension Actor {

    @nonobjc public class func fetchRequest() -> NSFetchRequest<Actor> {
        return NSFetchRequest<Actor>(entityName: "Actor")
    }

    @NSManaged public var name: String?
    @NSManaged public var profilePhoto: String?
    @NSManaged public var actorBio: String?
    

}
```
- Actor CoreDataClass 
```

import Foundation
import CoreData


public class Actor: NSManagedObject {
    convenience init(dictionary: [String:Any], context: NSManagedObjectContext)
    {
        if let ent = NSEntityDescription.entity(forEntityName: "Actor", in: context)
        {
            self.init(entity: ent, insertInto: context)
            self.name = dictionary["name"] as? String ?? ""
            self.profilePhoto = dictionary["profilePhoto"] as? String ?? ""
            self.actorBio = dictionary["actorBio"] as? String ?? ""

        }
            
        else
        {
            fatalError("Unable to find Entity name!")
        }
    }
}

```

- Actor ViewController

```
import UIKit

class ActorViewController {
    
    @IBOutlet weak var actorNameLabel: UILabel!
    @IBOutlet weak var actorImageView: UIImageView!
    
    var actorArray: [Actor]!
    
    actorArray = Actors.allObjects as? [Actors] ?? [Actors]()

    //Safe way to transfer the information form one view to other while tableView cell gets selected.
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        
        performOnMainthread {
            let actorDetailViewController = self.storyboard?.instantiateViewController(withIdentifier: "actorDetailViewController") as! ActorDetailViewController
            actorDetailViewController.selectedActor = self.actorArray[indexPath.row]
            self.navigationController?.pushViewController(actorDetailViewController, animated: true)
        }
    }
    			
}
```
- Actor DetailViewController

```
import UIKit

class ActorDetailViewController {
    
    @IBOutlet weak var actorNameLabel: UILabel!
    @IBOutlet weak var actorImageView: UIImageView!
    @IBOutlet weak var actorBio: UITextView!
    

	override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)
        
        actorNameLabel.text = selectedActor.name

        actorImageView.image = UIImage(named: selectedActor.profilePhoto)

        actorBio.text = selectedActor.actorBio

    }

```

### __Imagine that you have been given a project that has this GithubProjectViewController. The GithubProjectViewController should be used to display high-level information about a GitHub project. However, it’s also responsible for finding out if there’s network connectivity, connecting to GitHub, parsing the responses and persisting information to disk. It is also one of the biggest classes in the project.__

__How might you improve the design of this view controller?__

----

In the above description, it seems like that developer had put up all the content into a single file, which does not abide by the *Model View Controller* paradigm so I would refactor the coding standard and do the following steps. Doing so the code would be much more reusable and easier to test plus debug.
 - I would refactor the code into separate classes.  
 - I would create a model class for storing the data to a persistent store.  
 - I would also create a separate `GithubAPI` model class that handled the network requests and data parsing.  

### __If you were to start your iOS developer position today, what would be your goals a year from now?__

----

 - One year from now, I will have finished my Masters in Computer Science along with one or two more Nanodegrees, which will lead me to become a stronger software developer with a better understanding of the entire development process.
 - I would like to be in a challenging role that puts me in the position to expand my current skill set. I will use all of the new skills that I have learned and best practices to create exceptional products for the company that I work for.

 - I will be continuing my recent projects of creating cocoapods to generate great applications with minimal rework but maximize the total amount of work done.

 - It is apparent that the job I am apply for will help me to reach my goals.  The company exemplifies all of the things that I am most passionate about and their mission is exactly along the lines of my own. 
 
 - After a year of working at SmartThings, I will still be learning from my colleagues and applying all of the skills I gain to helping others to learn.  New inventions are released almost on regular bases and there is a lot of work that developers do behind the scene to make it happen. I am fascinated by new technological developments and to be able to push the limits of current boundaries will prove to be a fuel to keep working.

 - In summary, I will be continuing my professional and personal growth, using my newfound skills and applying the incredible passion that I have for all of the work that I do to make developers lives as easy as possible.
