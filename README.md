# 🚀 Go Multi-App GitOps Monorepo

Bu layihə, GitOps prinsiplərinə uyğun olaraq **GitHub Actions** və **Flux/ArgoCD** vasitəsilə idarə olunan çoxlu tətbiq (Monorepo) laboratoriyasıdır.

## 📁 Qovluq Strukturu

Layihə sənaye standartı olan Monorepo modelində qurulub:
* `.github/workflows/` - Bütün tətbiqləri idarə edən mərkəzi CI/CD pipeline.
* `apps/go-app/` - Go tətbiqinin kod bazası, Dockerfile-ı və K8s konfiqurasiyaları.
* `apps/go-app/base/` - Ortaq Kubernetes manifestləri (Deployment, Service).
* `apps/go-app/environments/` - Mühitlərə özəl (Dev/Prod) Kustomize sazlamaları.

---

## 🛠️ CI/CD Pipeline İşləmə Mexanizmi (Workflow)

Kod `main` budağına push və ya merge olunduqda, pipeline aşağıdakı mərhələləri icra edir:

1. **CI (Test & Build):** Go mühiti qurulur, testlər işlədilir. Uğurlu olarsa, unikal Git Commit SHA teqi ilə Docker image qurulub Registry-ə göndərilir.
2. **Deploy to DEV (Avtomatik):** Pipeline `apps/go-app/environments/dev/kustomization.yaml` faylındakı imaj teqini avtomatik yeniləyir və Git-ə push edir.
3. **Deploy to PROD (Təsdiqləmə ilə):** Pipeline Prod mərhələsində dayanıb **Manual Approval (Təsdiq Düyməsi)** gözləyir. GitHub-da təsdiq verildikdən sonra `apps/go-app/environments/prod/kustomization.yaml` faylı yenilənir.

---

## 🚀 Yeni Tətbiq (App) Əlavə Edilməsi

Layihəyə 2-ci və ya fərqli bir mikroservis əlavə etmək üçün:
1. `apps/` qovluğunun daxilində yeni bir qovluq açın (məsələn: `apps/payment-service/`).
2. Proqram kodlarını və `Dockerfile`-ı həmin qovluğun daxilinə yerləşdirin.
3. `base/` və `environments/` alt qovluqlarını yaradaraq Kustomize sazlamalarını daxil edin.
4. Mərkəzi pipeline hər bir tətbiqi dinamik olaraq öz qovluğu daxilində avtomatik tanıyıb idarə edəcəkdir.
