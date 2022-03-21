# go-what-is-my-pod-with-tracing

You need to launch this application on your Kubernetes cluster in a Pod containing an environment variable called `MY_POD_NAME`.

## Run locally

```
go run main.go
2022/03/01 09:56:21 Listening on localhost:8080
```

## Docker

```
docker build -t scraly/what-is-my-pod-with-tracing:1.0.2 .
docker run -it -p 8080:8080 scraly/what-is-my-pod-with-tracing:1.0.2
docker push scraly/what-is-my-pod-with-tracing:1.0.2
```

## Kubernetes

```
$ kubectl apply -f k8s/deployment.yml
deployment.apps/what-is-my-pod-with-tracing-deployment created

$ kubectl expose deployment what-is-my-pod-with-tracing-deployment --name=what-is-my-pod-with-tracing --type=LoadBalancer
service/what-is-my-pod-with-tracing exposed

$ stern what-is-my-pod-with-tracing
+ what-is-my-pod-with-tracing-deployment-7b85865ff-wlrvz › what-is-my-pod-with-tracing
what-is-my-pod-with-tracing-deployment-7b85865ff-wlrvz what-is-my-pod-with-tracing 2022/03/01 09:20:04 Listening on localhost:8080
```

## Test

```
$ export APP_URL=$(kubectl get svc what-is-my-pod-with-tracing -o jsonpath='{.status.loadBalancer.ingress[].ip}')
$ echo $APP_URL
$ curl http://$APP_URL:8080/
```