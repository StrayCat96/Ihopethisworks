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
        stage('Build a gradle project') {
          sh '''
          echo 'namespaces in the staging environment'
          kubectl get ns
          ls -l /var/secrets/google
          gcloud auth login --cred-file=$GOOGLE_APPLICATION_CREDENTIALS
          gcloud container clusters get-credentials hello-cluster --region us-west1 --project projectv2-381913
          echo 'namespaces in the prod environment'
          kubectl get ns
          '''
         }
        } 
       }
      }
     }
