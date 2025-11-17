@Library('jenkins-shared-library')

def configMap = [
    project : "roboshop",
    component : "catalogue"
]

if( ! env.BRANCH_NAME.equalsIgnoreCase('main') ){
     nodejsEKSPipeline(configMap) // by default it will call function inside thids pipeline
}
else{
    echo "please proceed with production process"
}
