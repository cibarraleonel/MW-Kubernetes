# Kubernetes - Comandos Útiles

## 🚀 Configuración Inicial

### Configurar kubectl con Kind
```bash
# Exportar configuración de kubeconfig
kind export kubeconfig --name <cluster-name>

# Verificar conexión al cluster
kubectl cluster-info

# Ver configuración actual
kubectl config view

# Ver contexto actual
kubectl config current-context
```

### Gestión de Namespaces
```bash
# Crear namespace
kubectl create ns <namespace-name>

# Configurar namespace por defecto
kubectl config set-context --current --namespace=<namespace-name>

# Listar namespaces
kubectl get ns
```

## 🔍 Inspección del Cluster

### Recursos y Objetos
```bash
# Ver tipos de recursos disponibles
kubectl api-resources

# Ver nodos del cluster
kubectl get nodes

# Ver nodos con más información
kubectl get nodes -o wide

# Ver todos los pods en todos los namespaces
kubectl get pods -A

# Ver pods con información del nodo
kubectl get pods -A -o wide

# Ver pods en namespace específico
kubectl get pods -n <namespace-name>
```

### Información Detallada
```bash
# Describir un objeto
kubectl describe <resource-type> <resource-name>

# Ver definición YAML de un objeto
kubectl get <resource-type> <resource-name> -o yaml

# Ver definición JSON de un objeto
kubectl get <resource-type> <resource-name> -o json

# Ver eventos del cluster
kubectl get events

# Ver eventos de un objeto específico
kubectl get events --field-selector=involvedObject.name=<object-name>,involvedObject.kind=<object-kind>
```

## 🛠️ Creación de Recursos

### Creación Imperativa
```bash
# Crear un pod
kubectl run <pod-name> --image=<image-name>:<tag>

# Crear un pod con variables de entorno
kubectl run <pod-name> --image=<image-name>:<tag> --env="<VAR_NAME>=<value>"

# Crear un pod con puerto expuesto
kubectl run <pod-name> --image=<image-name>:<tag> --port=<port-number>

# Agregar labels a un objeto
kubectl label <resource-type> <resource-name> <key>=<value>

# Remover label
kubectl label <resource-type> <resource-name> <key>-
```

### Creación Declarativa
```bash
# Aplicar un archivo YAML
kubectl apply -f <file.yaml>

# Aplicar múltiples archivos
kubectl apply -f <directory>/

# Aplicar desde URL
kubectl apply -f <url>

# Aplicar con kustomize
kubectl apply -k <directory>

# Ver qué se aplicaría sin ejecutar
kubectl apply --dry-run=client -f <file.yaml>

# Ver diferencias antes de aplicar
kubectl diff -f <file.yaml>
```

### Kustomize
```bash
# Ver salida de kustomize
kubectl kustomize <directory>

# Aplicar con kustomize
kubectl apply -k <directory>

# Ver diferencias con kustomize
kubectl diff -k <directory>
```

## 🔧 Gestión de Recursos

### Escalado y Actualización
```bash
# Escalar un deployment
kubectl scale deployment <deployment-name> --replicas=<number>

# Actualizar imagen de un deployment
kubectl set image deployment/<deployment-name> <container-name>=<new-image>

# Ver estado de rollout
kubectl rollout status deployment/<deployment-name>

# Ver historial de rollout
kubectl rollout history deployment/<deployment-name>

# Hacer rollback
kubectl rollout undo deployment/<deployment-name>
```

### Edición de Recursos
```bash
# Editar un recurso en vivo
kubectl edit <resource-type> <resource-name>

# Aplicar parche a un recurso
kubectl patch <resource-type> <resource-name> -p '<json-patch>'

# Reemplazar un recurso
kubectl replace -f <file.yaml>
```

## 🗑️ Eliminación de Recursos

```bash
# Eliminar un recurso específico
kubectl delete <resource-type> <resource-name>

# Eliminar usando archivo
kubectl delete -f <file.yaml>

# Eliminar usando kustomize
kubectl delete -k <directory>

# Eliminar todos los pods en el namespace actual
kubectl delete pod --all

# Eliminar recursos por label
kubectl delete <resource-type> -l <key>=<value>

# Forzar eliminación
kubectl delete <resource-type> <resource-name> --force --grace-period=0
```

## 🔌 Acceso a Aplicaciones

### Port Forwarding
```bash
# Hacer port-forward de un pod
kubectl port-forward pod/<pod-name> <local-port>:<pod-port>

# Hacer port-forward de un service
kubectl port-forward service/<service-name> <local-port>:<service-port>

# Port-forward en background
kubectl port-forward pod/<pod-name> <local-port>:<pod-port> &
```

### Proxy
```bash
# Crear proxy al API server
kubectl proxy

# Crear proxy en puerto específico
kubectl proxy --port=<port-number>
```

## 🐚 Ejecución de Comandos

### Acceso a Pods
```bash
# Ejecutar comando en un pod
kubectl exec <pod-name> -- <command>

# Ejecutar comando interactivo
kubectl exec -it <pod-name> -- /bin/bash

# Ejecutar en contenedor específico
kubectl exec -it <pod-name> -c <container-name> -- <command>

# Copiar archivos desde/hacia un pod
kubectl cp <pod-name>:<path> <local-path>
kubectl cp <local-path> <pod-name>:<path>
```

### Logs
```bash
# Ver logs de un pod
kubectl logs <pod-name>

# Ver logs en tiempo real
kubectl logs -f <pod-name>

# Ver logs de contenedor específico
kubectl logs <pod-name> -c <container-name>

# Ver logs anteriores (de contenedor que crasheó)
kubectl logs <pod-name> --previous

# Ver logs con timestamp
kubectl logs <pod-name> --timestamps

# Ver últimas N líneas
kubectl logs <pod-name> --tail=<number>
```

## 🏷️ Gestión de Labels y Nodos

### Labels
```bash
# Agregar label a un nodo
kubectl label nodes <node-name> <key>=<value>

# Ver labels de los nodos
kubectl get nodes --show-labels

# Seleccionar recursos por label
kubectl get <resource-type> -l <key>=<value>

# Ver recursos con selector complejo
kubectl get pods -l '<key1>=<value1>,<key2>=<value2>'
```

### Node Management
```bash
# Hacer un nodo no programable
kubectl cordon <node-name>

# Hacer un nodo programable
kubectl uncordon <node-name>

# Drenar un nodo (mover pods)
kubectl drain <node-name> --ignore-daemonsets
```

## 📊 Monitoreo y Debug

### Información del Sistema
```bash
# Ver uso de recursos
kubectl top nodes
kubectl top pods

# Ver información del cluster
kubectl cluster-info
kubectl cluster-info dump

# Ver versión de Kubernetes
kubectl version

# Ver configuración del cluster
kubectl config view
```

### Troubleshooting
```bash
# Ver eventos ordenados por tiempo
kubectl get events --sort-by='.metadata.creationTimestamp'

# Ver eventos en tiempo real
kubectl get events --watch

# Describir problema en un recurso
kubectl describe <resource-type> <resource-name>

# Ver estado de componentes del sistema
kubectl get componentstatuses
```

## 🔧 Comandos de Utilidad

### Autocompletado
```bash
# Habilitar autocompletado (bash)
source <(kubectl completion bash)

# Habilitar autocompletado (zsh)
source <(kubectl completion zsh)

# Alias útil
alias k=kubectl
```

### Explicación de Recursos
```bash
# Explicar campos de un recurso
kubectl explain <resource-type>

# Explicar campos específicos
kubectl explain <resource-type>.<field>

# Explicar recursivamente
kubectl explain <resource-type> --recursive
```

### Contextos y Configuración
```bash
# Ver contextos disponibles
kubectl config get-contexts

# Cambiar contexto
kubectl config use-context <context-name>

# Ver configuración actual
kubectl config view

# Ver configuración sin datos sensibles
kubectl config view --minify
```

