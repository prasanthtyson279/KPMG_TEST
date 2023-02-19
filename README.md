# KPMG_TEST

## 1. 3 Tier Architecture using MS Azure resources

Below are the Azure resources used for designing the architecture

1. Azure AD
2. Webapp
3. Web Application Firewall
4. Load balancer
5. Azure Kubernetes Services
6. Cosmos DB
7. Azure Keyvault
8. Azure Devops Pipelines

![image](https://user-images.githubusercontent.com/39862048/219966384-9098c5f4-70b4-4168-924c-292ce70bd54b.png)


## 2. Query the meta data of an instance within AWS or Azure or GCP and provide a json formatted output

Microsoft provides Azure Instance Metadata Service in order to retireve information of a VM which is not exposed to outside network

Below script has to be executed inside the VM
A non-routable IP address (169.254.169.254) is used inside the VM which always has the standard communication setup b/w the VM and IMDS service


**Windows**

Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance?api-version=2021-02-01" | ConvertTo-Json -Depth 64

**Linux**

curl -s -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2021-02-01" | jq

**Reference** : https://learn.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service?tabs=windows


## 3. We would like a function where you pass in the object and a key and get back the value.



def getKey(obj: dict):

    keys = list(obj)
    
    if len(keys) != 1:
    
        raise Exception('either multiple keys or empty dict found')
        
    else:
    
        return keys[0]



def getNestedValue(obj: dict, key: str, isFound = False):

    # print(obj, key, isFound)
    
    if type(obj) is not dict and not isFound:
    
        return None
        
    if (isFound or (key in obj.keys())) :
    
        if type(obj[key]) is dict:
        
            return getNestedValue(obj[key], getKey(obj[key]), True)
            
        else:
        
            # print(f'obj[getKey(obj)]: {obj[getKey(obj)]}')
            
            return obj[getKey(obj)]
            
    else:
    
        nestedKey = getKey(obj)
        
        return getNestedValue(obj[nestedKey], key, False)
        

if __name__ == '__main__':

    obj = {'a': {'b': {'c': 'd'}}}
    
    value = getNestedValue(obj, 'c')
    
    print(value)
    
