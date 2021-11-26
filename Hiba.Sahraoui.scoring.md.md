# Scoring CVSS

Nous allons reprendre les éléments du brief Qualifs qui seront la base de notre demande de calcul CVE pour l’adapter dans notre environnement :

Nous sommes dans une société située à Bern en Suisse, c'est une SS2I qui fournit des services dans l'IT. les investisseurs sont internationaux et vos enjeux également. Les contacts se situent en Islande à Reykjavik et également à Hong Kong.

AV= Adjacent Network
User Interection= Required
Scope= Changed
CIA= Low car c'est un environnement bienveillant

Métrique temporel RL= Official Fix
RC= Comfirmed

Les CVE suivantes sont liées au smartphone OPPO concerné par la propagation
du malware.

**CVE 2021-9583**

- Vector: 
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
- Base Score: 
9.8/10
- Le score calculé CVSS: 
4.2/10
- Lien: 
https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator?vector=AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H/E:X/RL:O/RC:C/CR:L/IR:L/AR:L/MAV:A/MAC:L/MPR:N/MUI:N/MS:C/MC:L/MI:L/MA:L&version=3.0

<u>Conclusion:</u>

Nous sommes dans un environnement bienveillant connectés en réseau adjacent, c'est à dire que les données sont stockées dans différents serveurs, il est moins facile de gérer les données à un plusieurs endroits.
La mise en place de cette sécurité a engendré une baisse du score CVSS "Common Vulnerability Scoring System" de 9.8/10 à 4.2/10, plus notre CVSS est faible, plus on considère qu'on est en sécurité. 
L'attaquant a plus de difficulté pour accéder à notre environnement ainsi que d'exploiter les données.
Attack Vector est un mode d'entrée dans un ordinateur ou un système en réseau, permettant à une personne ayant une intention malveillante d'endommager, de contrôler ou d'interférer de quelque manière que ce soit avec les opérations.
Les indicateurs d'impact, qu'on appelle CIA "Confidentiality, Intergrity, Availibility" sont des indicateurs faibles ce qui représente un impact faible sur les données stockées. 
La confidentialité signifie que les données, les objets et les ressources sont protégés contre les visualisations et autres accès non autorisés. L'intégrité signifie que les données sont protégées contre les modifications non autorisées afin de garantir leur fiabilité et leur exactitude. La disponibilité signifie que les utilisateurs autorisés ont accès aux systèmes et aux ressources dont ils ont besoin.
Vu que le score est à 4.2/10, on considère que nous sommes assez sécurisé pour une attaque.


**CVE 2021-11847**

- Vector: 
CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H
- Base Score: 
7.8/10
- Le score calculé CVSS: 
3.8/10
- Lien: 
https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator?vector=AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H/E:X/RL:O/RC:C/CR:L/IR:L/AR:L/MAV:A/MAC:L/MPR:L/MUI:R/MS:C/MC:L/MI:L/MA:L&version=3.0

<u>Conclusion:</u>

Nous sommes dans un environnement bienveillant connectés en réseau adjacent, ce qui justifie la baisse du score CVSS de 7.8/10 à 3.8/10. 
L'attaquant a plus de difficulté pour accéder à notre environnement ainsi que d'exploiter les données.
Les CIA sont des indicateurs faibles ce qui représente un impact sur les données stockées. 
Vu que le score est à 3.8/10, on considère que nous sommes assez sécurisé pour une attaque.


**CVE 2021-0930**

Cette CVE est assez particulière, en effet, Android a du faire sa mise à jour par rapport à cette vulnérabilité, les produits sont patchés, la vulnérabilité a été publiée le 01 novembre 2021, sur le site suivant : https://source.android.com/security/bulletin/2021-11-01.
En revanche, il est très tôt pour publier son CVSS sur National Vulnerability Database car ils attendent que tous le monde ait le temps de faire sa mise à jour avant de rendre cette vulnérabilité totalement publique et ainsi éviter un piratage donc finalement la version de la vulnérabilité est publique mais le détails est privé.


**CVE 2021-0918**

Idem que la CVE 2021-0930.

**CVE-2020-11830**

- Vector: 
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
- Base Score: 
9.8/10
- Le score calculé CVSS: 
4.2/10
- Lien: 
https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator?vector=AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H/E:X/RL:O/RC:C/CR:L/IR:L/AR:L/MAV:A/MAC:L/MPR:N/MUI:R/MS:C/MC:L/MI:L/MA:L&version=3.0

<u>Conclusion:</u>

Nous sommes dans un environnement bienveillant connectés en réseau adjacent, ce qui justifie la baisse du score CVSS de 9.8/10 à 4.2/10. 
L'attaquant a plus de difficulté pour accéder à notre environnement ainsi que d'exploiter les données.
Les CIA sont des indicateurs faibles ce qui représente un impact sur les données stockées. 
Vu que le score est à 4.2/10, on considère que nous sommes assez sécurisé pour une attaque.


**CVE-2021-42275**

- Vector: 
CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H
- Base Score: 
8.8/10
- Le score calculé CVSS: 
3.8/10
- Lien: 
https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator?vector=AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H/E:X/RL:O/RC:C/CR:L/IR:L/AR:L/MAV:A/MAC:L/MPR:L/MUI:R/MS:C/MC:L/MI:L/MA:L&version=3.0

<u>Conclusion:</u>

Nous sommes dans un environnement bienveillant connectés en réseau adjacent, ce qui justifie la baisse du score CVSS de 8.8/10 à 3.8/10. 
L'attaquant a plus de difficulté pour accéder à notre environnement ainsi que d'exploiter les données.
Les CIA sont des indicateurs faibles ce qui représente un impact sur les données stockées. 
Vu que le score est à 3.8/10, on considère que nous sommes assez sécurisé pour une attaque.


**CVE-2021-22941**

- Vector: 
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
- Base Score: 
9.8/10
- Le score calculé CVSS: 
4.2/10
- Lien: 
https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator?vector=AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H/E:X/RL:O/RC:C/CR:L/IR:L/AR:L/MAV:A/MAC:L/MPR:N/MUI:R/MS:C/MC:L/MI:L/MA:L&version=3.0

<u>Conclusion:</u>

Nous sommes dans un environnement bienveillant connectés en réseau adjacent, ce qui justifie la baisse du score CVSS de 9.8/10 à 4.2/10. 
L'attaquant a plus de difficulté pour accéder à notre environnement ainsi que d'exploiter les données.
Les CIA sont des indicateurs faibles ce qui représente un impact sur les données stockées. 
Vu que le score est à 4.2/10, on considère que nous sommes assez sécurisé pour une attaque.