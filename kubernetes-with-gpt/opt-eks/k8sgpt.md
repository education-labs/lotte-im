# k8sgpt

1. k8sgpt 설치

```
curl -LO https://github.com/k8sgpt-ai/k8sgpt/releases/latest/download/k8sgpt_amd64.deb
sudo dpkg -i k8sgpt_amd64.deb
```



2. 설치 확인

```
k8sgpt version
```



3. 지원 백엔드 모델 확인

```
k8sgpt auth list
```





```
curl -fsSL https://ollama.ai/install.sh | sh
```



```
ollama pull llama3.2:1b
```

```
k8sgpt auth add   --backend ollama   --model llama3.2:1b   --baseurl http://localhost:11434
```

```
k8sgpt analyze --explain --backend ollama --namespace default
```
