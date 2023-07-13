# Automation (Example -1)

### Creating an EventBridge rule that triggers a Lambda Function every hour which starts any stopped EC2 instances.

This can be used if at your company someone stops any EC2 instances for any reason and fails to start them at a later stage.

Here we will be creating an Cron job in EventBridge which runs every hour and triggers a Lambda function which checks for any stopped ec2 instances and starts them.


### We first create a Lambda Function...
![1](https://i.imgur.com/egF94Bh.jpg)

![2](https://imgur.com/skUt4UU.jpg)

![3](https://imgur.com/tQN4K3I.jpg)


### Our Lambda function needs proper permissions to talk to the EC2 instances
![5](https://imgur.com/DlTDCNm.jpg)

![6](https://imgur.com/hO5RdEg.jpg)

![7](https://imgur.com/3U9r0J5.jpg)

![8](https://imgur.com/zeOPjcE.jpg)

![9](https://imgur.com/jD4qVfu.jpg)

![10](https://imgur.com/kOPRlq2.jpg)

![11](https://imgur.com/HrXbg9y.jpg)

### Launch 3 EC2 instances
![13](https://imgur.com/pKDjqY7.jpg)


![13](https://imgur.com/99q7TLm.jpg)

```markdown
Once your EC2 instances are up, STOP 2 instances.
We will create a Test Event and run our Lambda function.
``````


### Test Event
![13](https://imgur.com/ujSpgS0.jpg)



### Lambda Function
```markdown
We are using boto3, an SDK for Python
```
```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.resource('ec2')
    for instance in ec2.instances.all():
        # print(instance.state)
        state = instance.state['Name']
        if state == 'stopped':
            instance.start()
            
```
![14](https://imgur.com/FBrvhyZ.jpg)
```markdown
Run the Lambda function and go to EC2 console.
Our 2 EC2 instance are now in the Running state... 
```

### Adding a trigger 
```markdown
We now create an EventBridge rule which will trigger this Lambda function every hour...
```
![15](https://imgur.com/UJtPrI2.jpg)

#### Here we create a Cron Expression - Cron(0/60 * * * ? *)
```markdown
Cron Expressions: [](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html)
```

![16](https://imgur.com/hM9zpnL.jpg)




