//This is a jenkins file that will pull from the master user.

pipeline {
  agent { node { label 'slave01' } } //agent any
  stages {
    stage('Clone Sources') {
        steps {
          checkout scm
        } 
     }
    stage('Executing bash script') {
      steps {
        sh '''
          if [ "$LANGUAGE" = "BASH" ] || [ "$LANGUAGE" = "ALL" ]; then
            cd ${WORKSPACE}/scripts/
            chmod 755 Bash_script.sh
            ./Bash_script.sh //*.sh
	    sh 'The output from the $LANGUAGE file is: ' > output.txt
            ./Bash_script.sh > output.txt
          else
            echo "$LANGUAGE file is selected! "
          fi
        '''
       }
      }     
      stage('Executing PYTHON script') {
         steps {
            sh '''
            if [ "$LANGUAGE" = "PYTHON" ] || [ "$LANGUAGE" = "ALL" ]; then
               cd ${WORKSPACE}/scripts/
               chmod 755 Python_script.py
               ${WORKSPACE}/scripts/Python_script.py $LANGUAGE
               sh 'The output from the $LANGUAGE file is: ' > output.txt
               ${WORKSPACE}/scripts/Python_script.py $LANGUAGE > output.txt
            else
               echo "$LANGUAGE file is selected! "
            fi
            '''
         }
      }
      stage('Executing C exe file') {
         steps {
            sh '''
              if [ "$LANGUAGE" = "C" ] || [ "$LANGUAGE" = "ALL" ]; then
                cd ${WORKSPACE}/scripts/
                chmod 755 C_script.c
                gcc C_script.c -o C.c
		./C.c 
                sh 'The output from the $LANGUAGE file is: ' > output.txt
		./C.c > output.txt
              else
                echo "$LANGUAGE file is selected! "
              fi
            '''
         }
      }      
      stage('Creating log file') {
        steps {
          sh '''
	    logFile = "${WORKSPACE}/logFile.txt"
            mkdir -p ${WORKSPACE}/              
            if [ -f "${logFile}" ]; then
                echo "A log file is already exists"
            else
	        touch ${logFile}
            fi              
	    cat ${WORKSPACE}/output.txt > ${logFile}
           '''
         }
      }
   }
}
