# Publicera på GitHub Pages

Projektet använder Astro 6.4.4 enligt package-lock.json och bygger statiska
sidor till dist/. Produktionsadressen är https://oliverochmatilda2027.se.
Ingen base med repository-namn ska anges eftersom domänen använder root (/).

## Skapa och koppla repository

1. Skapa ett tomt repository på https://github.com/new, exempelvis
   oliverochmatilda2027. Välj Public för GitHub Pages med GitHub Free.
   Lägg inte till README, .gitignore eller licens på GitHub.
2. Kör följande i PowerShell i projektmappen. Ersätt DITT-ANVANDARNAMN
   och använd exakt adressen till det repository du skapade:

   ```powershell
   git branch -m main
   git add .
   git diff --cached --stat
   git commit -m "Prepare wedding website for GitHub Pages"
   git remote add origin https://github.com/DITT-ANVANDARNAMN/oliverochmatilda2027.git
   git remote -v
   git push -u origin main
   ```

   Om Git ber om identitet, ange ditt namn och din GitHub-adress med
   `git config user.name "Ditt namn"` och `git config user.email "Din e-post"`,
   och kör sedan commit-kommandot igen.

3. Öppna repositoryts Settings → Pages. Under Build and deployment,
   välj GitHub Actions som Source.
4. Ange oliverochmatilda2027.se under Custom domain och spara.
   DNS-kontrollen kan inte lyckas förrän domänen pekar på GitHub Pages.
   DNS-ändringarna görs separat hos domänleverantören.
5. Öppna Actions → Deploy to GitHub Pages. Om första körningen misslyckades
   innan Pages aktiverades, välj Re-run all jobs eller Run workflow på main.
6. När DNS är klart och GitHub har utfärdat certifikatet, aktivera Enforce HTTPS
   i Settings → Pages och kontrollera sidan på produktionsadressen.

Varje push till main installerar låsta dependencies, kör npm run build,
laddar upp dist/ som Pages-artifact och deployar via GitHub Actions.
Workflow kan även startas manuellt. Ingen egen deploy-token behövs.

## Domän och CNAME

Vid ett eget GitHub Actions-workflow ignorerar GitHub Pages CNAME-filer.
Därför skapas ingen public/CNAME; Custom domain i Settings → Pages är
den styrande inställningen. Astro-konfigurationen anger produktionsadressen.
Domänen fungerar först när även DNS och HTTPS är klara. Repositoryts
standardadress under /repository-namn/ är inte avsedd som förhandsvisning
eftersom länkar och assets använder root.

## Lokal kontroll

Med Node 24 och npm 11:

```powershell
npm ci
npm run build
npm run preview
```

node_modules/, dist/, .astro/ och .env-filer ignoreras av Git.
package-lock.json ska committas. OSA använder det befintliga externa
Tally-formuläret och behöver ingen server på GitHub Pages.

Källor:
- https://v6.docs.astro.build/en/guides/deploy/github/
- https://github.com/withastro/action
- https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site
