default nginx image without using ecr
name: Deploy EKS Cluster
 
on:
  push:
    branches:
      - main
 
jobs: 
  deploy:
    runs-on: ubuntu-latest
 
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
 
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ap-south-1
 
    - name: Update kube config
      run: aws eks update-kubeconfig --name dp1-cluster --region ap-south-1
 
    - name: Deploy to EKS
      run: |
        kubectl apply -f k8s/deployment.yml --validate=false
        kubectl apply -f k8s/service.yml --validate=false
