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
          value: /var/secrets/google/week9project-381822-b2929df8ca6e.json
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
          gcloud container clusters get-credentials hello-cluster --region us-east1 --project week9project-381822
          echo 'namespaces in the prod environment'
          kubectl get ns
          '''
         }
        } 
       }
      }
     }
