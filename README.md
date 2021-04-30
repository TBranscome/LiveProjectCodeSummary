# LiveProjectCodeSummary

## Code Summary of Live Project from The Tech Academy

### Introduction and SignIn
For my final course in my time at The Tech Academy, I had to participate in a C# Live Project, which included other students and the instructors. We worked with Visual Studio, worked on front-end and back-end, and Azure. Daily stand-ups, and emails, reflected the work environment during a sprint, with code retrospectives and devOps. Working within a student built website, the project was based on a theatre that needed help with functionality and interfaces for user operation. The first part included adding your name to a signIn file on the website, showing that you can understand creating and merging branches using Visual Studio. 
### Reserve Story
After the initial SignIn portion, we were assigned a smaller story to get used to the feel of how the boards and work items in Azure worked. My reserve story included work on the about page. Working on the front-end side, my objectives were to fix some css issues with the top portion of the image in the header, the text within the image and centering of all the elements.

               <div class="contact-header text-center cms-bg-dark w-100 pb-5">
                <div class="aboutPageContainer">
                  <img class="card-img-top" src="@Url.Content("~/Content/images/theater.jpg")" alt="Card image cap">
                  <h3 class="centered aboutPage-h3">About Us</h3>
                </div>
                
Also, when sizing below a certain resolution, I had to fix the way the images shaped, as sizing down distorted the image container from circles to ovals, we did not want ovals. Also some hover functions were asked to be removed, so after going through, commenting out code, the about page was set.

### RentalHistory entity models and CRUD
After the reserve story, I was assigned to the rental history portion of the site. I was tasked with creating rental history pages which documented all rentals that were issued, including damage and a brief summary of the damage if any. I first needed to create entity models for rentlHistory, using a code first approach. After creating the model, the controller and views were created for Create Edit Details Delete and Index CRUD pages. 

    public class RentalHistory
      {
          public int RentalHistoryId { get; set; }
          public bool RentalDamaged { get; set; }
          public string DamagesIncurred { get; set; }
          public string Rental { get; set; }
      }
 
### Create User HistoryManager
After creating the CRUD pages for rentalHistory, I was tasked with creating a user, the rental history manager, which would have further properties, like how many users were restricted and how many rental replacement requests were issued, all being extended from the main ApplicationUser. The HistoryManager was not added to the dBcontext to properly establish a Table per Hierarchy for the other classes. Multiple classes, one table.

    public class HistoryManager : ApplicationUser
        {
            public int RestrictedUsers { get; set; }
            public int RentalReplacementRequests { get; set; }
        }
      
### Seeding the History Manager 
This was the portion of the Live Project that required an Admin type user, the HistoryManager, to be added to the database, which would later have authorization access for the CRUD pages Edit/Delete/Create. Used this code to determine if the role of HistoryManager existed, and if it did not then one would be created, and given the proper credentials to login and access the rental history portion of the database.

        public static void HistoryManagerSeed(ApplicationDbContext context)
        {
            var roleManager = new RoleManager<IdentityRole>(new RoleStore<IdentityRole>(context));
            var userManager = new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(context));

            if (!roleManager.RoleExists("HistoryManager"))
            {
                var role = new IdentityRole();
                role.Name = "HistoryManager";
                roleManager.Create(role);

                
                var historyManager = new HistoryManager
                {
                    UserName = "historyManager",
                    Email = "historyManager@theatrevertigo.com",
                    RestrictedUsers = 47,
                    RentalReplacementRequests = 123
                };
                string historyManagerPass = "vertigoHistoryManager";

                var checkHistoryMan = userManager.Create(historyManager, historyManagerPass);
                
                if (checkHistoryMan.Succeeded)
                {
                    userManager.AddToRole(historyManager.Id, "HistoryManager");
                }

            }
        }

### Styling the Create/Edit pages
For the styling of the pages, I had to write code in the css and javascript files of the rental area. Built a javascript function that changed the form labels if the rental was damaged or not. If damaged, the label for "Notes" would change to "Damages Incurred". 

    var rentHistoryCheckBox = document.querySelector('.check-box');
    var rentHistoryNotesLabel = document.querySelector('.rentalHistory-notes-label');



    function isDamaged() {
        if (rentHistoryCheckBox.checked) {
            rentHistoryNotesLabel.innerHTML = "Damages Incurred";

        }
        else {
            rentHistoryNotesLabel.innerHTML = "Notes";

        }
    }

    if (document.getElementById("rightPage") !== null) {
        window.onload = isDamaged();
    }
Also, using css, I would keep everything centered and keep the max width at 600px.

    .rentalHistory-container {
        display:flex;
        justify-content:center;
    }

    .rentalHistory-Form{
        max-width: 600px;
        color: var(--light-color);
        text-align: left;
        background-color: var(--main-color);
        border-radius: 20px;
        padding-top: 10px;
        font-weight: bolder;
    }

    .rentalHistory-Btn {
        display:flex;
        justify-content:center
    }
    
## Skills Learned
* Using proper version control. Creating new branches from the master, which would be updated daily, and learning how to save changes. Commiting and pushing branches and creating pull requests when merging branches to the master.
* Improving on how to work as a team on a project. Using resources together to solve problems, like the Slack app for communication, and also the daily Stand ups with the project manager.
* How to use Azure DevOps to maintain work items and boards. 
* Basic understanding of how a development team works and operates through a sprint. Sprint planning, stand ups, code retrospectives. Just the overall planning that goes into a project and how its broken down for ease and function.
* Improved knowledge of MVC and CRUD.
