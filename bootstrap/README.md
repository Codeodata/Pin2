Ajustar variables.tf con propio nombre

variable "name_of_s3_bucket" {
  description = "Name of S3 bucket for terraform state"
  type        = string
  default     = "agustin-gonzales.mundose22"
}
