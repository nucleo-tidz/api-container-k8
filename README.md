# Azure Container registry
A smple workflow to push dot net 6 API into azure container regitry

### Prerequiste
- Azure container registry is provisoned in azure
- Service principal is created

### Create Service Principal
 Service Principal will be used to login into container registry
 ```shell
az login
az account set --subscription=<subscrition id>
az acr show --name <container regisry name> --query "id"  
az ad sp create-for-rbac --name "api://acr-service-principal" --scopes <output of above step> --role owner 
```

### Action Steps
```shell
    - name: Login container registry        
      uses: docker/login-action@v2.1.0
      with:
          registry: xyz.azurecr.io #Server URL of container registry
          username: <service principal client id>
          password: <service principal client secret>
    
    - name: Push to container registry
      run: |
        docker build -f ${{ env.docker-file-path }} . -t xyz.azurecr.io/nucleotidz/api:${{github.sha}}         
        docker push xyz.azurecr.io/nucleotidz/api:${{github.sha}}
        #Mandatory to Follow this Pattern  <Server URL of container registry>/<repository name of your choice>/<>unique image name> for in above commands
        ```
    

