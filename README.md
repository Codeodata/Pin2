# 📌 Introducción

Este proyecto automatiza la implementación de una infraestructura de servidor web en AWS utilizando Terraform. Incluye un proceso de inicialización para la gestión del estado de Terraform y la implementación principal de la infraestructura para un servidor web Apache básico.

# 🚀 Características principales

### Implementación automatizada de infraestructura

### Gestión del estado con S3 y DynamoDB

### Integración con GitHub Actions

### Servidor web Apache básico

# ⚙️ Guía de Configuración

🔹 Inicialización del Backend de Estado

cd bootstrap
terraform init
terraform plan
terraform apply

🔹 Esto crea:

Un bucket de S3 para almacenar el estado de Terraform.

Una tabla de DynamoDB para el bloqueo del estado.

🔹 Implementación de la Infraestructura

La infraestructura se implementa automáticamente mediante GitHub Actions. En cada push a la rama main que incluya cambios en el directorio infrastructure/, el workflow realizará:

Inicialización de Terraform

Validación de la configuración

Aplicación de los cambios

# 🔹 Para una implementación manual:

* cd infrastructure
* terraform init
* terraform plan
* terraform apply

## Verificación de Recursos

# 🔹 Verificación del Backend de Estado

## Verificar el bucket de S3
aws s3 ls | grep bucket-terraform-lab

## Verificar la tabla de DynamoDB
aws dynamodb list-tables | grep terraformstatelock

# 🔹  Verificación de la Infraestructura

## Verificar la VPC
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=terraform-vpc"

## Verificar la instancia EC2
aws ec2 describe-instances --filters "Name=tag:Name,Values=webserver" "Name=instance-state-name,Values=running"

## Probar el servidor web
curl http://$(terraform output -raw Webserver-Public-IP)
