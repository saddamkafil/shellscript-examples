### Below shellscript curls the airflow url and check for scheduler health and puts the log in logs file 

```
$ cat test1.sh
#!/bin/bash
while true; do
time=`date +"%Y-%m-%d-%T"`
Log="monitor.log"


value=`curl -s -X GET http://airflow.com/health -H "Accept: application/json" | sed -n '6p' | cut -c 20-28` ##replace http://airflo.com/health with your airflow url
value2=unhealthy
value1=$value
mystring=$value1
mystring2=$value2

#value=`curl -s -X GET http://api.cdp.bridg.com/health -H "Accept: application/json" | grep unhealthy`
#value=`curl -s -X GET http://api.cdp.bridg.com/health -H "Accept: application/json" | grep healthy | sed -n '2p' | cut -c 20-28`
#value=`curl -s -X GET http://api.cdp.bridg.com/health -H "Accept: application/json" | sed -n '3p' | cut -c 20-26`
#value=`curl -s -X GET http://api.cdp.bridg.com/health -H "Accept: application/json" | sed -n '6p' | cut -c 20-28`

if [ "$mystring" == "$mystring2" ]; then
echo "$time Scheduler is Down" >> $time_$Log
curl -X POST -H 'Content-type: application/json' --data '{"text":"airflow-1 is DOWN"}' https://hooks.slack.com/services/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
#sleep 60
else
echo "$time Scheduler is up" >> $time_$Log
curl -X POST -H 'Content-type: application/json' --data '{"text":"airflow-1 is UP"}' https://hooks.slack.com/services/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
echo "$time sleep started 60 sec" >> $time_$Log
sleep 60
echo "$time sleep ended 60 sec" >> $time_$Log
fi
echo "$time moniter started for evry 5 sec" >> $time_$Log
sleep 5
echo "$time moniter sleep ended for 5 sec" >> $time_$Log
done
```
```
