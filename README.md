Jekins set up and real time using commands ]
JENKINS: Its a CI/CD TOOL.

CI : CONTINOUS INTEGRATION
CD : CONTINOUS DELIVERY/DEPLOYMENT

INTEGRATION : ADDING (SOUCE CODE)
INTEGRATE (ADDING) THE OLD CODE WITH THE NEW CODE.

CI : CONTINOUS BUILD + CONTINOUS TEST

BEFORE CI LIMITATIONS:
1. TIME CONSUMING
2. MANUAL WORK
3. COMPLEXITY 

CI : BUILD + TEST + DEPLOY (OPT)

CD: CONTINUOUS DELIVERY/DEPLOYMENT

CONTINUOUS DELIVERY : Deploying the code into production manually.
CONTINUOUS DEPLOYMENT: Deploying the code into production Automatically.


PIPELINE: 
Series of events interlinked with each other.
Step by step execution of a particular process. 


ENV : 
1. NON-PROD : DEV, TEST  (INTERNAL PURPOSE)
2. UAT      : USER ACCEPTANCE TESTING (CLIENT)
3. PROD     : LIVE SERVER (USERS) 

Every app we are using now is Deployed on PROD(LIVE) sever 


JENKINS: 
Jenkins is an open source project written in java by Kohsuke Kawaguchi. 
it runs on the Window, Linux and Mac OS.
It is Community supported.
Every thing is going to be worked on  plugins.
Automates the Entire Software Development Life Cycle (SDLC).

HUDSON -- > SUN MICRO SYSTEM(2004) -- > ORCALE -- > HUDSON=JENKINS, PAID=FREE

SETUP: 5 STEPS (8080 -- > OPEN IN SG) --- > ALL TRAFFIC (ANYWHERE)

STEP-1: GETTING REPO & KEY (jenkins.io)
  sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
  
STEP-2: INSTALLING JAVA-11/17, GIT, MAVEN, JENKINS
  yum install java git maven jenkins -y

SETP-3: START JENKINS SERVICE
   systemctl start jenkins.service
   systemctl status jenkins.service
Note: By default any service will be on stopped state, we need to start it.

STEP-4: CONNECTING TO JENKINS
  Copy-public-ip/dns of jenkin server:8080
  EX: http://52.90.84.94:8080/
  
STEP-5: CONFIGURE

http://52.90.84.94:8080/


INTEGRATION OF GIT WITH JENKINS:

NEW ITEM -- > FREE STYLE (NAME) -- > OK -- > GIT -- > 
build -- > console output -- > green color
cd /var/lib/jenkins/workspace/JOB-1

Dahboard -- > job -- > condigure -- > Build step -- > Invoke top-level Maven targets
mvn compile -- > save 

green   : success
red     : failed
blue    : processing
black   : stopped
orange  : some depends missing

CUSTOM WROKSPACE: getting our outputs on our required locations on server.

cd 
mkdir -p swiggy/dailybuilds
cd /
chown jenkins:jenkins /root/ -R

dashboard -- > job -- > general -- > advance -- > Use custom workspace -- > 
/root/swiggy/dailybuilds -- > save

EXECUTING LINUX COMMANDS:

Build steps -- > execure shell command -- > 

================================================================================================================================
DAY-02:
Installig jenkins from script:

vim jenkins.sh

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
yum install java git maven jenkins -y
systemctl start jenkins.service
systemctl status jenkins.service

sh jenkins.sh

PARAMETERS: Passing the information. 

in real time we will get our works on form of tickets.
Ticketing tools: JIRA, SHAREPOINT, TRELLO, SERVICE NOW, WORKDAY.

TYPES:
CHOICE : You can provide the choices.
Dashboarh -- > raham -- > Configuration -- > This project is parameterized -- > choice

STRING : we can pass the values directly without specifiying.
Dashboarh -- > raham -- > Configuration -- > This project is parameterized -- > string

MULTI-STRING: To pass the input in multiple lines  
Dashboarh -- > raham -- > Configuration -- > This project is parameterized -- > multistring

FILE: To build the local files.
Dashboarh -- > raham -- > Configuration -- > This project is parameterized -- > file

BOOLEAN: Defines true or false.
Dashboarh -- > raham -- > Configuration -- > This project is parameterized -- > Boolean


VARIABLES: Varies as per the time.  used to hold the values

1. User defined variables
2. Jenkins Environment variables


User defined variables:
a. local  : it will work inside the job only.
b. global : it will work inside & outside of the job also.

a. local variable: ex-1:
name=raham
loc=hyderabad
echo "hai all my name is $name, im from $loc"

b. global variables: 

Dashboard  -- > Manage Jenkins -- > Configure System -- > Global properties -- > Environment variables -- > add 

when we define a value locally and globally, the local value will be taken by jenkins.

2. Jenkins Environment Variables: variables which will change as per the build.

user defined vs jenkins env vars
user defined vars will not chanaged, jenkins env vars will change.
users cannot defined the jenkins vars.

==========================================================================================================================

Throttle build: we can restrict the number of builds per hour/day/weekk ----

================================================================================================

DAY-03:

PASSWORD LESS LOGIN:

vim /var/lib/jenkins/config.xml  (7 th line true=false)
systemctl restart jenkins.service
systemctl status jenkins.service
public-ip:8080

=======================================================================================

CRON JOB: Used to schedule the jobs at particual time intervals.

*	: minutes
*	: hours
*	: date
*	: month
* 	: day (sun=0, sat=6)

9:00 am 1 june 2023	:	00 9 1 6 4
8:56 pm 31 05 2023	:	56 20 31 5 3
				56 20 31 5 3
new job -- > free style -- > git : url -- > Build Triggers  -- > Build periodically -- > (* * * * *) -- > save

shortcut: https://crontab-generator.org/
Build Triggers 
9am -- > source code is not chnaged -- > cronjob -- > build -- > trigger -- > waste

=======================================================================================

POLL SCM: Will make a build when the developer changes the code in given time.
if the code is chnaged on given time then only it will make a build.

new job -- > free style -- > git : url -- > Build Triggers  -- >poll scm -- > (* * * * *) -- > save

Disadvantge:
we give time interval for 1 hr -- > dev 5 mins -- > 55 minutes (waste of time)
in 1 hr i chnaged the code 2 times but i will get only last chnaged code.

=======================================================================================

WEBHOOK: Will trigger the build immedieatly when dev change the code.

github -- > repo -- > settings -- > webhooks -- > add webhook -- > password -- > Payload URL: jenkinsurl(public:8080) 

ex: (http://54.87.7.81:8080/github-webhook/)
Content type : application/json  -- > add webhook

new job -- > free style --> git : url -- > Build Triggers  -- > GitHub hook trigger for GITScm polling -- > save 

=========================================================================================

LINKED JOBS: J0b-1 is linked with job-2.
if i build job-1 then j0b-2 will trigger automatically.

1. UP STREAM JOBS
2. DOWN STREAM JOBS

post build: Actions that are happening after build.

create 2 jobs -- > job-1 -- >  Post-build Actions -- > build other projects -- > job2 -- > save

=========================================================================================

Permalinks: shows the build infromation and status as well.

Last build (#5), 18 sec ago
Last stable build (#5), 18 sec ago
Last successful build (#5), 18 sec ago
Last completed build (#5), 18 sec ago

============================================================================================

Execute concurrent builds if necessary :  When this option is checked, multiple builds of this project may be executed in parallel. 

=========================================================================================

TIME STAMP: Add the timestamps to console output.
it will be helpful in troubleshooting.

Troubleshooting	: Finding solution for the issue.

job1 -- > configure -- > Build Environment -- > Add timestamps to the Console Output -- > shell

echo "the code is building"
sleep 30
echo "the code is testing"

save

In real time every log file will have the timestamp enabled.
Log file : used to store the events happend on server.

==================================================================


DAY-04:

PIPELINE: Step by step execution of a Particular process.
Series of events interlinked with each other.

WHY TO USE PIPELINE INSTED OF FREE STYLE ?
in pipeline you will visual of execution.

TYPES:
1. SCRIPTED 
2. DECLARATIVE (MAJORLY)


note: to execute linux commands we need to give sh 

SCRIPTED:

SINGLE STAGE: WE have only one stage in this pipieline.
MULTI STAGE: WE have more than one stage in this pipieline.

node {
    stage("stage1") {
        sh 'touch file1'
    }
    stage("raham") {
        sh 'touch file2'
    }
}

DECLARATIVE:

pipeline:
agent	: on which server the pipeline is executing.
stages	: to define the stages 
stage	: what you are performing
steps	: action you are performing


pipeline {
    agent any 
    
    stages {
        stage("create file") {
            steps {
                sh 'touch file3'
            }
        }
        stage('users list') {
            steps {
                sh 'cat /etc/passwd'
            }
        }
        stage('memory useage') {
            steps {
                sh 'lsmem'
            }
        }
    }
}

PIPELINE AS A CODE: Executing multiple commands over a single stage


pipeline {
    agent any 
    
    stages {
        stage('abc') {
            steps {
                sh 'touch file4'
                sh 'cat /etc/passwd'
                sh 'lsmem'
            }
        }
    }
}


MULTI STAGE PAAC:

pipeline {
    agent any 
    
    stages {
        stage('abc') {
            steps {
                sh 'touch file4'
                sh 'cat /etc/passwd'
                sh 'lsmem'
            }
        }
        stage('def') {
            steps {
                sh 'cat /etc/group'
                sh 'ifconfig'
            }
        }
    }
}

=====================================================================================

PIPELINE AS A CODE OVER SINGLE SHELL:

pipeline {
    agent any 
    
    stages {
        stage('abc') {
            steps {
                sh '''
                touch file4
                cat /etc/passwd
                lsmem
                '''
            }
        }
    }
}

===========================================================================================

variables: (local & global)

pipeline {
    agent any 
    environment {
        name='raham'
        loc='hyderabad'
        clinet='nareshit'
    }
    
    stages {
        stage('abc') {
            steps {
               sh 'echo my name is $name, im residing on $loc, im working for $clinet'
            }
        }
    }
}

JENKINS DEFINED VARS:

pipeline {
    agent any 
    
    stages {
        stage('abc') {
            steps {
               sh 'echo the build number is $BUILD_NUMBER'
            }
        }
    }
}

PRINTENV: To print all the jenkins variables

pipeline {
    agent any 
    
    stages {
        stage('abc') {
            steps {
               sh 'printenv'
            }
        }
    }
}

=========================================================================================

PARAMETERS: 

CHOICE:

pipeline {
   agent any
   
       parameters {
           choice(name: 'server', choices: ['a','b','c'], description: "")       
       }
   stages {
       stage('parameters') {
           steps {
               echo 'this is my prod stage'
           }
       }
   }
}


BOOLEAN:

pipeline {
   agent any
       parameters {
           booleanParam(name: 'db', defaultValue: true, description: "")
       }
   stages {
       stage('parameters') {
           steps {
               echo 'this isn my prod stage'
           }
       }
   }
}

STRING:

pipeline {
   agent any
    environment {
           name = 'raham'
       }
       parameters {
           string(name: 'person', defaultValue: 'raham shaik', description: "how are you")
       }
   stages {
       stage('parameters') {
           steps {
               sh 'echo "${name}"'
               sh 'echo "${person}"'
           }
       }
   }
}

===========================================================================================

CI PIPELINE:

pipeline {
    agent any
    
    stages {
        stage('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RAHAMSHAIK007/11ambatch2023.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}


=====================================================================================================


DAY-06:

MASTER & SLAVE :

When build is triggered all the output is on master only.
50 GB --- > 49 GB memory --- > new builds -- > Distribute the Build --- > JENKINS MASTER & SLAVE 


LIMITATIONS:
1. MEMORY COSUMPTION
2. SERVER LOAD 
3. BUILDS UNAVAILABLE 
4. TIME CONSUMING

SLAVE : it is also a server, which is communicating with master (SSH).
It will store the build output.
we can create multiple slave for jenkins.

LABEL : used to Assingn build to worker node.
AGENT : to work with slave server we need to install an agent (java-11/17)

STEP-1: CREATE A SLAVE SERVER 
STEP-2: INSTALL AGENT (yum install java -y)
STEP-3: CONFIGURE YOUR SLAVE ON JENKINS DASHBOARD

Dashboard  -- > Manage Jenkins -- > Manage Nodes & Clouds  -- > New node

Node name: slave1  -- > Permanent Agent -- > create

Number of executors	:	3	: Number of builds parallely
Remote root directory	:	/tmp	: custom workspace on slave
Labels			:       dev	: Assingn build to worker node
Usage			: 	last	: when the build need to execute
Launch method		:       last	: how master & slave communicate
Host: private-ip-worker node, 
creds -- > add -- > jenins -- > kind: ssh user name with private key 
username: ec2-user & Private Key: use ppk of workernode -- > enter directly -- > add 
Host Key Verification Strategy : last 


AMAZON LINUX 2022:

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
amazon-linux-extras install java-openjdk11 -y 
yum install git maven jenkins -y
systemctl start jenkins.service
systemctl status jenkins.service



AMAZON LINUX 2023:

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
yum install java git maven jenkins -y
systemctl start jenkins.service
systemctl status jenkins.service

==================================================================================================================================


pipeline {
    agent {
        label 'prod'
    }
    
    stages {
        stage('one') {
            steps {
                git branch: 'main', url: 'https://github.com/RAHAMSHAIK007/11ambatch2023.git'
            }
        }
    }
}

==============================================================================

RBAC	: ROLE BASED ACCESS CONTROL
To restrict the jenkins Control for the users.

user-1: EXP           user-2:FRESHER	USER-3: FRESHER

1. USERS: Dashboard -- > Manage Jenkins -- > security -- > Users (crearte 2 users)
2. PLUGIN: Dashboard -- > Manage Jenkins -- > Plugins -- > Available plugins -- > Role-based Authorization Strategy  -- > install -- > goback to top
3. Dashboard  -- > Manage Jenkins -- > Security -- > Authorization: Role-Based strategy
4. ROLES : Dashboard  -- > Manage Jenkins -- > Security -- > Manage and Assign Roles -- >
   create Fresher & Experience roles and give permissions(EXP: ADMIN, FRE: BUILD AND READ)
5. ASSIGN ROLES: user-1: EXP           user-2:FRESHER

==============================================================================
LIST VIEW: USED TO SEPERATE/FILTER THE JOBS.
MY VIEW: USED TO SHOW THE JOBS WHICH I HAVE PERMISSIONS.
BUILD PIPELINE VIEW: To show the process of linked jobs -- > PLUGINS -- > AVAILABLE -- > Build Pipeline -- > INSTALL

BLUE OCEAN: used to have good visusla for our dashboard.

PLUGIN: Dashboard -- > Manage Jenkins -- > Plugins -- > Available plugins -- > Blue Ocean -- > install # jenkins-all-commands
Jenkins all real time useful commads
