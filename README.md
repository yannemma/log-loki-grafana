# Loki
Les étapes ci-dessous aideront à installer Loki pour collecter les journaux du cluster et les envoyer à Grafana pour analyse.

# 1. Installer Kube-Prometheus-Stack sur ubuntu:
Cette commande Helm met à jour ou installe le chart "kube-prometheus-stack" provenant du référentiel "prometheus-community" 
dans l'espace de noms "monitoring", en utilisant les valeurs personnalisées fournies dans 
le fichier "grafana-config.yaml", avec le nom de sortie "prod":  

    helm upgrade --install prod prometheus-community/kube-prometheus-stack -n monitoring --values grafana-config.yaml

# 2. Installer Promtail  
Ces commandes Helm ajoutent le référentiel Helm Grafana, mettent à jour les référentiels Helm disponibles, 
puis installent ou mettent à jour le chart "promtail" provenant du référentiel "grafana"
dans l'espace de noms "monitoring", en utilisant les valeurs personnalisées fournies dans le fichier "promtail-config.yaml".  

    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update
    helm upgrade --install promtail grafana/promtail --values promtail-config.yaml -n monitoring

# 3. Installer Loki  
Cette commande Helm met à jour ou installe le chart "loki-distributed" provenant 
du référentiel "grafana" dans l'espace de noms "monitoring" avec le nom de sortie "loki".  

    helm upgrade --install loki grafana/loki-distributed -n monitoring

# 4. Validate the logs on Grafana:
Cette commande Kubernetes permet de faire un port forwarding du service "prod-grafana" situé dans l'espace de noms "monitoring",
 en redirigeant le trafic du port 3000 de votre machine locale vers le port 80 du service "prod-grafana". 

    kubectl port-forward service/prod-grafana -n monitoring 3000:80
