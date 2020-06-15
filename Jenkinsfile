//This is a jenkins file that will pulled from the master user.

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
          if [ "$LANGUAGE" = "Bash" ] || [ "$LANGUAGE" = "All" ]; then
            cd ${WORKSPACE}/scripts/
            chmod 755 *.sh
            ./bashscript.sh 
            ./bashscript.sh > results
          else
            echo "$LANGUAGE file is selected! "
          fi
        '''
       }
      }     
      stage('Executing Python script') {
         steps {
            sh '''
            if [ "$LANGUAGE" = "Python" ] || [ "$LANGUAGE" = "All" ]; then
               cd ${WORKSPACE}/scripts/
               chmod 755 *.py
               ${WORKSPACE}/scripts/python.py //$LANGUAGE
               sh 'The output from the $LANGUAGE file is: ' > output.txt
               ${WORKSPACE}/scripts/python.py $LANGUAGE > output.txt
            else
               echo "$LANGUAGE file is selected! "
            fi
            '''
         }
      }
      stage('Executing C exe file') {
         steps {
            sh '''
              if [ "$LANGUAGE" = "C" ] || [ "$LANGUAGE" = "All" ]; then
                cd ${WORKSPACE}/scripts/
                chmod 755 *.c
                gcc c_script.c -o c_script
				        ./c_script 
                sh 'The output from the $LANGUAGE file is: ' > output.txt
				        ./c_script > output.txt
              else
                echo "$LANGUAGE file is selected! "
              fi
            '''
         }
      }      
      stage('Saving Log file') {
        steps {
          echo 'Saving log file process..'
          sh '''
	          log_file="${HOME}/Documents/ProjectLog/log"
            mkdir -p ${HOME}/Documents/ProjectLog/              
            if [ -f "${log_file}" ]; then
                echo "file ${log_file} exists"
            else
	              touch ${log_file}
            fi              
            echo "Build Number $BUILD_NUMBER" >> ${log_file}
            cat ${WORKSPACE}/scripts/results >> ${log_file}
	          echo "#############################" >> ${log_file}
           '''
         }
      }
   }
}
