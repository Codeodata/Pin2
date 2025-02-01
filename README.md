Introducción

Este proyecto automatiza la implementación de una infraestructura de servidor web en AWS utilizando Terraform. Incluye un proceso de inicialización para la gestión del estado de Terraform y la implementación principal de la infraestructura para un servidor web Apache básico.

Características principales:

Implementación automatizada de infraestructura.

Gestión del estado con S3 y DynamoDB.

Integración con GitHub Actions.

Servidor web Apache básico.

Guía de Configuración

1. Inicialización del Backend de Estado

cd bootstrap
terraform init
terraform plan
terraform apply

Esto crea:

Un bucket de S3 para almacenar el estado de Terraform.

Una tabla de DynamoDB para el bloqueo del estado.

2. Implementación de la Infraestructura

La infraestructura se implementa automáticamente mediante GitHub Actions. En cada push a la rama principal que incluya cambios en el directorio infrastructure/, el workflow realizará:

Inicialización de Terraform.

Validación de la configuración.

Aplicación de los cambios.

Para una implementación manual:

cd infrastructure
terraform init
terraform plan
terraform apply

Verificación de Recursos

Verificación del Backend de Estado

# Verificar el bucket de S3
aws s3 ls | grep nelson-rios.mundose22

# Verificar la tabla de DynamoDB
aws dynamodb list-tables | grep terraformstatelock

Verificación de la Infraestructura

# Verificar la VPC
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=terraform-vpc"

# Verificar la instancia EC2
aws ec2 describe-instances --filters "Name=tag:Name,Values=webserver" "Name=instance-state-name,Values=running"

# Probar el servidor web
curl http://$(terraform output -raw Webserver-Public-IP)

Variables de Configuración

Backend de Estado (bootstrap/variables.tf)

name_of_s3_bucket: Nombre del bucket de S3 para almacenar el estado.

dynamo_db_table_name: Nombre de la tabla de DynamoDB para bloqueo del estado.

Infraestructura (infrastructure/variables.tf)

aws_region: Región de AWS para la implementación.

vpc_cidr: Bloque CIDR de la VPC.

subnet_cidr: Bloque CIDR de la subred.

instance_type: Tipo de instancia EC2.

ami_id: ID de la AMI para el servidor web.

Estructura del Proyecto

Componentes de la Infraestructura

bootstrap/

main.tf: Crea el bucket de S3 y la tabla de DynamoDB para la gestión del estado.

variables.tf: Variables de configuración del backend de estado.

infrastructure/

backend.tf: Configuración del backend de Terraform.

main.tf: Configuración de la instancia EC2.

setup.tf: Configuración de la VPC y la red.

setup_apache.sh: Script de instalación de Apache.

variables.tf: Variables de configuración de la infraestructura.

CI/CD

.github/workflows/

TerraformApply.yml: Workflow para la implementación automática.

TerraformDestroy.yml: Workflow para la eliminación de la infraestructura.


