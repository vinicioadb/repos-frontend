# Catálogo frontend — reto DevOps Easysoft

Frontend estático del catálogo. En Kubernetes, Nginx reenvía `/api/*` al Service interno `repos-api:3000`.

## Validación local

Con el backend activo en el puerto 3001:

```powershell
npm ci
npm run test:coverage
$env:API_PORT='3001'
npm start
```

Abre `http://localhost:8080`. El reporte para SonarQube Cloud se genera en `coverage/lcov.info`.

## Configuración de GitHub y SonarQube Cloud

El workflow `.github/workflows/ci.yml` ejecuta pruebas, cobertura, análisis Sonar y una construcción Docker. En GitHub configura:

- Secret de Actions: `SONAR_TOKEN`.
- Variable de Actions: `SONAR_ORGANIZATION`.
- Variable de Actions: `SONAR_PROJECT_KEY`.

Mientras falte alguno, únicamente se omite Sonar; las demás verificaciones siguen funcionando.

## Minikube

```powershell
minikube image build -t repos-front:1.0 .
helm upgrade --install repos-front ./helm -n reto
kubectl port-forward -n reto svc/repos-front 8080:80
```
