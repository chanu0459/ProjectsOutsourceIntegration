# ProjectsOutsourceIntegration

Outsource - Projects Outsource Integration 
 
**Outsource**
  1. User clicks on the Project Management link
     - New tab will be opened with the casecamp link and auto login auth code 

    Dev 
      method : project_management_path
      helper : ApplicationHelper
      usage  : views/layouts/_sidebar.html.erb
 
  2. On signup / login we will check for API key
      - API key present? - We will login the user and create project management link
      - API key not present? - We will create a user in projects_outsource and store the api_key and enc_api_key in database

    Dev
      SignUp
        method     : configure_okta
        controller : Devise::RegistrationsController
        sub_method : new_user
        lib        : ProjectManagementIntegration

      Login
        method      : create
        controller  : Devise::SessionsController
        sub_method  : create_api_user
        model       : User

  
  3. Project Creation API , Default Case creation API, Default Comment creation API, Assign Freelancer to Project
      - Request will be sent to Casecamp with the api_key and project details to create a project.

    Dev
      method     : change_bid_status
      controller : BidsControllere
      sub_method : casecamp_integration
      model      : Job
      The above sub method is responsible for Project Creation, First Case ccreation , First comment creation and Assigning Freelancer to Project
       Under the Hood
        lib      : ProjectManagementIntegration
        methods  : new_project, add_user_to_project, default_case, default_comment
 
Projects Outsource
 
  1. User trying to Login form Outsource. 
      - API will be exposed to Login 
        - Accept enc_api_key  
        - on succcessful login Projects Outsource page will be displayed, if failed redirected to root path of Projects.
      
      Dev 
        method      : authorize
        controller  : HomeController
   
  2. Project Creation API , Default , Default , Assign Freelancer to Project
      - API will be exposed to create a project from Outsource

    Dev
      method     : new_project
      controller : Outsource::V1::ProjectsController 
      sub_method : default_time_category, default_time_budget

  3. Case creation API
      - API will be exposed to create a Case from Outsource

    Dev
      method     : default_case
      controller : Outsource::V1::CasesController 
      sub_method : default_time_category, default_time_budget 

  4. Comment creation API
      - API will be exposed to create a Comment from Outsource      

    Dev
      method     : default_comment
      controller : Outsource::V1::CommentsController 
      sub_method : comment_params

  5. User creation API
      - API will be exposed to create a User from Outsource      

    Dev
      method     : new_user
      controller : Outsource::V1::UsersController 
      sub_method : generate_api_key
