# kops-kubernetes-cluster

github sayfasından release indirilir.

```https://github.com/kubernetes/kops```

daha sonra cloud shell üzerinden işlem yapabiliriz ya da kendi sunucunuzdan.

```gcloud auth application-default login```

unique bir bucket yaratmamız gerekiyor.

```gsutil mb -l europe-north1 gs://my-kops-clusters-111/```

cluster kurulumu için gerekli variablelar

```
PROJECT=`gcloud config get-value project`

export KOPS_FEATURE_FLAGS=AlphaAllowGCE

export KOPS_STATE_STORE=gs://my-kops-clusters-111/
```

cluster için config oluşturuyoruz öncelikle.

```kops create cluster simple.k8s.local --node-count 3 --zones europe-north1-a --state ${KOPS_STATE_STORE}/ --project=${PROJECT}```

özelliklerini değiştirmek istiyorsak onları değiştiriyoruz.

```kops edit ig --name=simple.k8s.local nodes-europe-north1-a
kops edit ig --name=simple.k8s.local master-europe-north1-a
```


ve en son cluster ı kuruyoruz

```kops update cluster --name simple.k8s.local --state ${KOPS_STATE_STORE} --yes --admin```

cluster kurduktan sonra kubeconfiği aşağıdaki komutlar çekeriz.

```kops export kubecfg simple.k8s.local --state ${KOPS_STATE_STORE} --admin```

