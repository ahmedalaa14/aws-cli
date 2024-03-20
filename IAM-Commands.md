# same way as ec2 had a bunch of commands for components relevant for ec2 instances, iam does too
    aws iam create-group --group-name MyIamGroup
    aws iam create-user --user-name MyUser
    aws iam add-user-to-group --user-name MyUser --group-name MyIamGroup
# verify that my-group contains the my-user
    aws iam get-group --group-name MyIamGroup
# attach policy to group
## this is the command so we need the policy-ARN - how can we get that?
aws iam attach-user-policy --user-name MyUser --policy-arn {policy-arn} - attach to user directly
    aws iam attach-group-policy --group-name MyGroup --policy-arn {policy-arn} - attach policy to group
    ## let's go and check on UI AmazonEC2FullAccess policy ARN
## OR if you know the name of the policy 'AmazonEC2FullAccess', list them
    aws iam list-policies --query 'Policies[?PolicyName==`AmazonEC2FullAccess`].{ARN:Arn}' --output text
    aws iam attach-group-policy --group-name MyGroup --policy-arn {policy-arn}
# validate policy attached to group or user
    aws iam list-attached-group-policies --group-name MyGroup - [aws iam list-attached-user-policies --user-name MyUser]
# Now that user needs access to the command line and UI, but we didn't give it any credentials. So let's do that as well!
## UI access
    aws iam create-login-profile --user-name MyUser --password My!User1Login8P@ssword --password-reset-required
    -> user will have to update password on UI or programmatically with command: aws iam
    update-login-profile --user-name MyUser --password My!User1ADifferentP@ssword
# Create test policy
    aws iam create-policy --policy-name bla --policy-document file://bla.json
## cli access
    aws iam create-access-key --user-name MyUser
    -> you will see the access keys