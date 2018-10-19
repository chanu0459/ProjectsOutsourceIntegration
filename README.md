# ProjectsOutsourceIntegration

**Initial Setup**

 1. Drop Database of projects.outsource.com
 2. Seed
 3. Create Owner from projects.outsource.com UI
 4. Run the following in console
  - user = User.find_by_email "`owner email`"
	 - user.update_attribute(:status_active, true)
	 - company = user.company
	 - company.update_attribute(:skip_subscription, true)
	
  - api_key =  loop do
     - token = SecureRandom.base64.tr('+/=', 'Qrt')
     - break token unless User.exists?(api_key: token)
     - end
  - user.update_attribute(:api_key, api_key)
 5. store api_key in outsource site_configs
  - name : os_api_key
 6. Open outsource console and run
	 - User.all.each do |user|
		  - user.create_api_user
	 - end


**Outsource - Projects Outsource Integration - Manual**
 
**Outsource**
  1. User clicks on the Project Management link
     - The list of projects will be displayed with same design as CaseCamp.

    Dev 
      Controller  : ProjectManagementController
      method      : index
      Api method  : ProjectManagementIntegration / get_projects
      url path    : /projects
 
  2. On click of any project Cases View will be displayed
      
    Dev
      Controller  : ProjectManagementController
      method      : cases
      Api method  : ProjectManagementIntegration / get_cases
      url path    : /projects/:id/cases

  
  3. On click of any case CaseDetails view will be displayed

    Dev
      Controller  : ProjectManagementController
      method      : case_details
      Api method  : ProjectManagementIntegration / case_details
      url path    : /projects/:id/case_details
  
  4. Adding Case API - In Cases view page user can create new Case  
    
    Dev
      Controller  : CaseOperationsController
      method      : add_case
      Api method  : ProjectManagementIntegration / new_case
      url path    : /projects/add_case

  
  5. Adding Comment API - In caseDetails page user can add comments with attachments
  
    Dev
      Controller  : ProjectManagementController
      method      : add_comment
      Api method  : ProjectManagementIntegration / default_comment
      url path    : /projects/add_comment
  
  6. History Page - In Case View page, on the right top you can find the button to get the history of the case.
  
    Dev
      Controller  : ProjectManagementController
      method      : history
      Api method  : ProjectManagementIntegration / get_history
      url path    : /projects/history?code=
      
   7. My Comments - In Case View page, on the right top you can find the button to get the comments of the user for the project.
  
    Dev
      Controller  : ProjectManagementController
      method      : user_comments
      Api method  : ProjectManagementIntegration / user_comments
      url path    : /projects/user_comments?code=
   
   8. Chart - In caseDetails page user can add comments with attachments
  
    Dev
      Controller  : ProjectManagementController
      method      : gantt_chart
      Api method  : ProjectManagementIntegration / gantt_chart
      url path    : /projects/gantt_chart?code=
    
   9. Priority Page - In caseDetails page user can add comments with attachments
  
    Dev
      Controller  : CaseOperationsController
      method      : priority
      Api method  : ProjectManagementIntegration / priority
      url path    : /projects/:id/priority
 
**Projects Outsource**
  
  1. Project Creation API , Default Time Category , Default Time Budget , Assign Freelancer to Project
      - API will be exposed to create a project from Outsource
    
    Dev
      method     : new_project
      controller : Outsource::V1::ProjectsController 
      sub_method : default_time_category, default_time_budget
  
  2. Project List API
      - API will be exposed to get list of projects for the logged in user.
    
    Dev
      method     : projects_list
      controller : Outsource::V1::ProjectsController 
      sub_method : ProjectsIndex / projects
  

  3. Case creation API
      - API will be exposed to create a Case from Outsource

    Dev
      method     : new_case / default_case
      controller : Outsource::V1::CasesController 
      sub_method : default_time_category, default_time_budget 
  
  4. Case List API
    
    Dev
      method     : cases_list
      controller : Outsource::V1::CasesController 
      sub_method : CasesIndex / cases

  5. Comment creation API
      - API will be exposed to create a Comment from Outsource      

    Dev
      method     : default_comment
      controller : Outsource::V1::CommentsController 
      sub_method : comment_params
   
  6. Case Details API
     
    Dev
      method     : case_details
      controller : Outsource::V1::CommentsController 
      sub_method : CasesIndex / case_show

  7. User creation API
      - API will be exposed to create a User from Outsource      

    Dev
      method     : new_user
      controller : Outsource::V1::UsersController 
      sub_method : generate_api_key
  
  8. History Page - In Case View page, on the right top you can find the button to get the history of the case.
  
    Dev
      Controller  : Outsource::V1::HistoryController
      method      : index
      
  9. My Comments - API to fetch logged in user comments.
  
    Dev
      Controller  : Outsource::V1::CommentsController
      method      : user_comments
   
  10. Chart - In caseDetails page user can add comments with attachments
  
    Dev
      Controller  : Outsource::V1::ProjectsController
      method      : gantt_chart
      sub method  : ProjectsIndex / gantt_chart
    
  11. Priority Page - API for list of cases priority wise.
  
    Dev
      Controller  : Outsource::V1::CommentsController
      method      : priority
      sub method  : CasesIndex / cases_priority
