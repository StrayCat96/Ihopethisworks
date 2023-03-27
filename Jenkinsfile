podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: cloud-sdk
        image: google/cloud-sdk
        command:
        - sleep
        args:
        - 9999999
        volumeMounts:
        - name: shared-storage
          mountPath: /mnt
        - name: google-cloud-key
          mountPath: /var/secrets/google
        env:
        - name: GOOGLE_APPLICATION_CREDENTIALS
          value: /var/secrets/google/projectv2-381913-9b0d3109d8df.json
      restartPolicy: Never
      volumes:
      - name: shared-storage
        persistentVolumeClaim:
          claimName: jenkins-pv-claim
      - name: google-cloud-key
        secret:
          secretName: sdk-key
''') {
  node(POD_LABEL) {
    stage('Deploying to prod') {
      container('cloud-sdk') {
        stage('Deploy calculator') {
          sh '''
          echo 'namespaces in the staging environment'
          kubectl get ns
          gcloud auth login --cred-file=$GOOGLE_APPLICATION_CREDENTIALS
          gcloud container clusters get-credentials hello-cluster --region us-west1 --project projectv2-381913
          git clone 'https://github.com/StrayCat96/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition'
          cd Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition/Chapter08/sample1
          chmod +x ./kubectl
          ./kubectl apply -f calculator.yaml -n devops-tools
          ./kubectl apply -f hazelcast.yaml -n devops-tools
          '''
         }
        } 
       }
      }
     }
