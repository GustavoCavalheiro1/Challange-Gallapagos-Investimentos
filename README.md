git init
git add eneva_agk_dashboard.html
# Renomeie para index.html — obrigatório para o Pages funcionar
mv eneva_agk_dashboard.html index.html
git add index.html
git commit -m "feat: ENEVA AGK financial dashboard"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/eneva-agk-dashboard.git
git push -u origin main
