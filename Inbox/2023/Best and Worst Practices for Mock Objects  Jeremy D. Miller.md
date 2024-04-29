---
created: 2023-10-09T08:30:00 (UTC -03:00)
tags: []
source: https://web.archive.org/web/20210920211948/http://codebetter.com/jeremymiller/2006/01/10/best-and-worst-practices-for-mock-objects/
author: Posted by
---

# Best and Worst Practices for Mock Objects | Jeremy D. Miller

> ## Excerpt
> Mock objects are like any other tool.  Used one way they’re a huge time saver.  In other cases they can easily create more work and degrade the quality of the testing code.  In this post I’ll try to present some best and worst practices for utilizing mock objects.  Sadly, every “bad” practice described is something I’ve either done or seen done.

---
Mock objects are like any other tool.  Used one way they’re a huge time saver.  In other cases they can easily create more work and degrade the quality of the testing code.  In this post I’ll try to present some best and worst practices for utilizing mock objects.  Sadly, every “bad” practice described is something I’ve either done or seen done.

  

This is the third in a planned series of five posts on mock objects:

  
  

  
2.  [Introduction to Mocks and Stubs](https://web.archive.org/web/20210920211948/http://codebetter.com/blogs/jeremy.miller/archive/2005/12/19/135757.aspx)
  
4.  [When and Why to use Mocks and Stubs](https://web.archive.org/web/20210920211948/http://codebetter.com/blogs/jeremy.miller/archive/2005/12/20/135800.aspx)
  
6.  Best Practices for Mocks (this post)
  
8.  Constraints and other Mocking Tricks
  
10.  Attaching Mocks and Stubs with StructureMap (by request)

  

**Mocking Interfaces and Classes outside Your Code**

  

Be careful about mocking or stubbing any interface that is outside your own codebase.  If you don’t truly understand the semantics and proper usage of an interface you don’t have any business mocking that interface in a unit test.  My best advice is to create your own interface to wrap the interaction with the external API classes. 

  

The best example from my memory was a security subsystem that retrieved security authorization and user information from Active Directory.  We created a class that queried Active Directory directly (with a painful detour through ADAM) and wrote tests that were fully connected.  Technically we could have probably mocked the System.DirectoryServices classes somehow and just verified that we used the correct LDAP query command, but I don’t think it would have added any value.  The reason I think a mock would have been useless is that determining the correct LDAP query string to return the correct results was the majority of the effort.  I can write SQL queries in my sleep and XPath expressions when I’m merely drowsy, but the LDAP query syntax is pretty wacky (and poorly documented in my experience).

  

Instead of trying to mock the access to Active Directory, create your own coarse-grained interface for retrieving user information that will encapsulate the AD access.  Besides the testability gain, it’ll keep your other security code decoupled from the storage so that you can chuck the Active Directory strategy when your aggravation level gets too high.  This interface is safe to mock.

  

      public interface IUserInformationStore

      {

            UserInformation FindUser(string userName);

            UserInformation\[\] FindUsersInRole(string roleName);

      }

  
  

**Avoid Mocking Fine-Grained or Chatty Interfaces**

  

Don’t mock fine grained interfaces.  You’ll wear off the skin on your fingertips and the tests will NOT be intention revealing. 

  

Specifically, do not mock happy, funny ADO.Net (or JDBC for the Java crowd).  Just think of how many different ADO.Net objects and lines of code it takes to make a typical call to the database.   I’ve watched a couple of people try to write unit tests on their data access code by mocking IDbConnection, IDbCommand, and the various IDataParameter objects.  It’s too much effort for too little gain.  My advice is to just test the data access code against the actual database with integration tests.  For the most part I think testing persistence code without the database is a complete waste of time.  I’d make an obvious exception for data access frameworks and code that generates sql commands though.

  

By all means, mock the database in your unit tests, but [do it in the right way](https://web.archive.org/web/20210920211948/http://codebetter.com/blogs/jeremy.miller/archive/2005/10/12/133017.aspx).  With the exception of a DataSet (maybe), I don’t think you should ever expose any ADO.Net objects in the public interface that your data access classes expose to the rest of the system.

  

If you have a chatty interface, see if you can bunch the calls into some sort of coarse parameter object.  Fowler describes this in his Refactoring book as the [Introduce Parameter Object](https://web.archive.org/web/20210920211948/http://www.refactoring.com/catalog/introduceParameterObject.html) refactoring.  As an example, consider the case of testing user interface screen logic in an ASP.Net application.  One of the first things you need to do is isolate as much code as possible from the ASP.Net runtime objects (you can create an HttpContext in a unit test, but it’s the blackest magic).  We can’t mock the Session directly, but we can put an interface in front of the Session to do state management.

  

      public struct SessionConstants

      {

            internal const string SORT\_KEY = “Sort”;

            internal const string NAME\_FILTER\_KEY = “NameFilter”;

            internal const string PAGE\_NUMBER\_KEY = “PageNumber”;

            internal const string DATASET\_KEY = “Data”;

      }

  

      public interface ISession

      {

            string GetStringValue(string key);

            void SetStringValue(string key, string sessionValue);

  

            DataSet GetDataSetValue(string key);

            void SetDataSetValue(string key, DataSet data);

      }

  

      public class SessionWrapper : ISession

      {

            private HttpSessionState \_session;

  

            public SessionWrapper()

            {

                  \_session = HttpContext.Current.Session;

            }

  

            public string GetStringValue(string key)

            {

                  return (string) \_session\[key\];

            }

  

            public void SetStringValue(string key, string sessionValue)

            {

                  \_session\[key\] = sessionValue;

            }

  

            public DataSet GetDataSetValue(string key)

            {

                  return (DataSet) \_session\[key\];

            }

  

            public void SetDataSetValue(string key, DataSet data)

            {

                  \_session\[key\] = data;

            }

      }

  

We can now test any web page controller class by using a mock object as a substitute for the Session.  The ISession and SessionWrapper pair will definitely make unit testing the web page controller classes easier, but it forces us into a lot of setup calls in the unit tests for each piece of state (page number, sorting expression, user preferences, etc.).  I think it also creates an ugly coupling from the controllers to the storage mechanism and structure.  A better approach in my opinion is to create a coarse grained class to hold the web page state and a new interface for retrieving the state management that completely decouples the actual storage mechanism.

  

      public class PageState

      {

            private string \_sort;

            private string \_filter;

            private int \_pageNumber;

            private DataSet \_data;

  

            public PageState(string sort, string filter, int pageNumber, DataSet data)

            {

                  \_sort = sort;

                  \_filter = filter;

                  \_pageNumber = pageNumber;

                  \_data = data;

            }

  

            public string Sort

            {

                  get { return \_sort; }

                  set { \_sort = value; }

            }

  

            public string Filter

            {

                  get { return \_filter; }

                  set { \_filter = value; }

            }

  

            public int PageNumber

            {

                  get { return \_pageNumber; }

                  set { \_pageNumber = value; }

            }

  

            public DataSet Data

            {

                  get { return \_data; }

                  set { \_data = value; }

            }

      }

  
  

      public interface IStateService

      {

            PageState RetrieveState();

            void StoreState(PageState state);

      }

  

Now all it takes to setup the mock object in a unit test for a controller class is an expectation for the RetrieveState() method and another for the StoreState() method.  The session state for the test can be quickly setup by calling the constructor function on the PageState object.

  

In a comment to my last post Ayende Rahein pointed out that excessive numbers of fine-grained expectations will make the tests very brittle to changes in the code.  Been there, done that.

  

**How Many Mock Objects in any Given Test?**

  

How many mock objects are too many?  I’ve worked with some people before that felt that there should never be more than 1-2 mock objects involved in any single unit test.  I wouldn’t make a hard and fast rule on the limit, but anything more than 2 or 3 should probably make you question the design.  The class being tested may have too many responsibilities or the method might be too large.  Look for ways to move some of the responsibilities to a new class or method.  Excessive mock calls can often be a sign of poor encapsulation. 

  

Inline with [Jim Shore’s Reflective Design](https://web.archive.org/web/20210920211948/http://www.jamesshore.com/Blog/Reflective-Design.html) writing, unit tests and mock objects are another means to gather feedback about your code and design.  If you’re having difficulty using a mock object in a unit test, or you’re incurring more setup overhead in your tests than you’d like, it’s frequently a sign that you need to reconsider your class design.  In other words, too much mocking, either in the number of mock objects or the setup code, in any single unit test is a Coding Smell.

  

Minimizing the number of mock objects necessary for any given unit test is a very effective way of keeping the [Cyclomatic Complexity](https://web.archive.org/web/20210920211948/http://codebetter.com/blogs/raymond.lewallen/archive/2005/06/13/64527.aspx) of your classes below an acceptable level.

  
  

**Only Mock your Nearest Neighbor**

  

Ideally you only want to mock the dependencies of the class being tested, not the dependencies of the dependencies.  From hurtful experience, deviating from this practice will create unit test code that is very tightly coupled to the internal implementation of a class’s dependencies.  A common culprit is the diabolical usage of the Singleton pattern (the authors of the GoF book are making a second version that will supposedly have a full section on Singleton abuse).  A frequent usage of a Singleton is to create a Repository object that manages the communication between a client and its backend services.  Since you cannot mock a Singleton (yeah I know you can do some IL magic, but do you really want to?), myself and many other people fall into the trap of attaching a mock for the backend data source classes into the Singleton instance and setting up the expectations on this mock.  Your unit test is basically validating that the calls that the class being unit tested makes to the repository causes another set of calls to a mock object that is standing in for the data service.  If the Singleton is in an unknown state or the internal caching mechanisms of the repository change, you **will** end up rewriting unit tests.

  

Since all I really wanted to test in this case was the interaction of a class with the repository class, I’m much better off writing a simpler test that mocks the repository directly.  Then I don’t have to worry about the state or the internals of the concrete implementation of the repository.

  

As long as you’re aware of the consequences of a Singleton or static members in general, it’s relatively simple to create a repository that doesn’t harm testability.  Here’s a couple of links on this specific issue:

  
  

  
-   [http://codebetter.com/blogs/jeremy.miller/articles/129545.aspx](https://web.archive.org/web/20210920211948/http://codebetter.com/blogs/jeremy.miller/articles/129545.aspx)
  
-   [http://codebetter.com/blogs/jeremy.miller/archive/2005/08/04/130302.aspx](https://web.archive.org/web/20210920211948/http://codebetter.com/blogs/jeremy.miller/archive/2005/08/04/130302.aspx)
  
-   [http://structuremap.sourceforge.net/SingletonInjection.htm](https://web.archive.org/web/20210920211948/http://structuremap.sourceforge.net/SingletonInjection.htm)

  

**Mock the Right Thing**

  

Here’s an example of the single dumbest thing I’ve ever done with mock objects (and also an example of how NOT to do Model View Presenter).  We were building a WinForms application with somewhat complex workflow between the screens.  Each screen had some logic in the Presenter (Controller) class that determined whether or not the screen could be closed, usually by checking if the data on the screen was “dirty.”  To simulate a view with dirty data, we did something like the following:

  

      \[TestFixture\]

      public class PresenterTester

      {

            \[Test\]

            public void CloseViewWhenViewIsNotDirty()

            {

                  // Create the mock objects

                  IMock msgBoxMock = new DynamicMock(typeof(IMessageBoxCreator));

                  IMock viewMock = new DynamicMock(typeof(IView));

  

                  // Define the expected interaction

                  msgBoxMock.ExpectNoCall(“AskYesNoQuestion”, typeof(string), typeof(string));

  

                  // Setup the view so that the controller will think the screen is dirty

                  viewMock.SetupResult(“Name”, “Jeremy”);

                  viewMock.SetupResult(“PhoneNumber”, “SOMETHING DIFFERENT TO MAKE THE SCREEN DIRTY”);

                  viewMock.SetupResult(“Email”, “jeremy@somewhere.com”);

                  viewMock.SetupResult(“State”, “TX”);

                  viewMock.SetupResult(“PostalCode”, “78750”);

  
  
  

                  viewMock.Expect(“Close”);

  

                  Model model = new Model(“Jeremy”, “555-555-5555″, “jeremy@somewhere.com”, “TX”, “78750”);

  

                  // Create the presenter class with the mock objects

                  Presenter presenter = new Presenter(

                        model,

                        (IView) viewMock.MockInstance,

                        (IMessageBoxCreator) msgBoxMock.MockInstance);

  

                  // Perform the unit of work

                  presenter.Close();

  

                  // Verify the interaction

                  msgBoxMock.Verify();

                  viewMock.Verify();

            }

      }

  

This is a terrible unit test.  The “dirty” state of the view was tested by comparing the value of each property on the view class compared to the same property on the internal Model member of the Presenter class.  The unit tests setup “clean” or “dirty” states by setting up results for every single property on the IView interface.  In practice this obfuscated the intention of the unit tests and made the unit tests more laborious to write and maintain.  Everytime you added a new field to the screen, all the Presenter unit tests would break. 

  

In this case, isolate the “dirty” check so it can be tested separately from the screen flow logic.  What I do now is put an IsDirty() method on the view interface.  In the Presenter unit tests I simply setup a value for IsDirty() to easily simulate all of the paths through the code. 

  

            \[Test\]

            public void CloseViewWhenViewIsNotDirty()

            {

                  // Create the mock objects

                  IMock msgBoxMock = new DynamicMock(typeof(IMessageBoxCreator));

                  IMock viewMock = new DynamicMock(typeof(IView));

  

                  // Define the expected interaction

                  msgBoxMock.ExpectNoCall(“AskYesNoQuestion”, typeof(string), typeof(string));

  

                  // Very simply set the screen to be in a “clean” state

                  viewMock.ExpectAndReturn(“IsDirty”, false);

                  viewMock.Expect(“Close”);

  

                  // Create the presenter class with the mock objects

                  Presenter presenter = new Presenter(

                        (IView) viewMock.MockInstance,

                        (IMessageBoxCreator) msgBoxMock.MockInstance);

  

                  // Perform the unit of work

                  presenter.Close();

  

                  // Verify the interaction

                  \_msgBoxMock.Verify();

                  \_viewMock.Verify();

            }

  

 Now we’ve got a lot less mock setup and the unit tests will be less brittle and easier to understand because we’ve eliminated a lot of the noise from the view property setup. 

  

**Watch Out When You Mock Abstract Classes**

  

I would urge a certain level of caution in mocking an abstract class or a concrete class.  For best results my advice is to only mock interfaces to avoid unwanted side effects.  It is certainly possible to mock an abstract class, doing that leads to some side effects that effectively negate all of the advantages of a mock object.  Dynamic mock object tools like NMock can only override virtual methods.  This might not be so much a problem in other languages, but with the .Net languages all methods are non-virtual by default.  This means that some of the behavior that you were trying to remove from the test with a mock is still there.  The dynamic mock libraries work by using Reflection.Emit to create a dynamic proxy on the fly for the requested interface or abstract class.  In the case of an abstract class the dynamic mock can only intercept virtual members and even worse, the constructor function of the superclass will probably be exercised.  Here’s an example of mocking an abstract class that’s caused me some grief in the past.

  

      public abstract class ServiceProvider

      {

            public ServiceProvider()

            {

                  // Connect to a web service to cache some data

            }

  

            public abstract void PerformService();

  

            // Cannot be intercepted by a mock object because it’s non-virtual

            public DataSet LookupData(string id)

            {

                  return new DataSet();

            }

      }

  
  

      \[TestFixture\]

      public class MockExampleTestFixture

      {

            \[Test\]

            public void TestSomething()

            {

                  // As soon as this code is executed the constructor function of

                  // ServiceProvider will be called

                  DynamicMock mock = new DynamicMock(typeof(ServiceProvider));

            }

      }

  

When you try to create a mock for ServiceProvider the abstract class is trying to access a web service in its constructor function.  If the web service isn’t available, the unit test blows up.  Even with the mock object, your unit test is now using the web service, making an implicit dependency between your unit test and an external resource and effectively eliminating the advantage of using a mock object in the first place.  The solution is might be simple; look for an opportunity to do an [Extract Interface](https://web.archive.org/web/20210920211948/http://www.refactoring.com/catalog/extractClass.html) refactoring to create an IServiceProvider interface that can be mocked without any side effects.
