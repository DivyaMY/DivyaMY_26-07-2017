Prepare a Jenkins pipeline job (DSL / groovy script) to satisfy following CI-Cd pipeline :
•	Send approval email to Release Manager after the application packaging phase 
•	Trigger the Jenkins job  on reception of the approval email for the above job .

Solution:
For the Above question can be achieved by
->Assuming Approval_deploymemt job is for sending approval mail to release manager after the application packaging phase.
->and Deployment job is for Trigger the jenkins job on receltion of the approval email for the above job.


1) jenkins should have plugins installed.
  -promotion plugin
2) jenkins jobs.
   -Approval_deployment
   -Deployment
   
  --->Setup Approval_deployment Job
  --->Configure Approval_deployment Job to send email to your "approver" as part of Email Ext post-build action or script.
  --->Configure the email to contain link back to the job run (not just job name, or you could even link directly to promotion from the email)
DSL Script 
****************************************************************
job('example') {
    publishers {
        extendedEmail {
            recipientList('divya@xyz.org')
            defaultSubject('Approval')
            defaultContent('Something broken')
            contentType('text/html')
            triggers {
                beforeBuild()
                stillUnstable {
                    subject('Approval Mail')
                    content('Hi,
                             Application packaging is done.
                             Please provide the Approval for job 
                             to Run by clicking following link
                             below <Promotion URL>')
                    sendTo {
                        developers()
                        requester()
                        culprits()
                    }
                }
            }
        }
    }
}
**************************************************************************
 --->Configure a Promotion on Approval_deployment Job 
 --->In that promotion, allow it to be run only by your "approver" user (by name)
 --->Configure that promotion to trigger Deployment job

When Approval_deployment is run, it will send email to "approver". He/she will click the link and come to the Jenkins job run UI. He/she should be logged in to Jenkins with their "approver" user.

Then he/she can click the promotion star and simply click "approve" on it. This will trigger the promotion which in turn triggers Deployment job

