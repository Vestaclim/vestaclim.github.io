# Vesta Almanach

Bilans climatiques quotidiens d'une maison ancienne en pierre meulière à
Trilport (Seine-et-Marne) — l'almanach du foyer : chaque matin, un rapport
raconte ce que la maison a fait de sa nuit, ce que le ciel prépare, et les
deux ou trois gestes qui comptent.

**Lecture** : [vestaclim.github.io](https://vestaclim.github.io) — dernier
bilan, calendrier d'historique, et pour chaque journée le rapport complet
téléchargeable (`.md`) dont la page HTML est la lecture du matin.

## Comment ces bilans sont produits

Un **LLM orchestré localement** rédige chaque bilan. Il n'applique pas un
gabarit : il observe, calcule, puis écrit. Ses trois sources, interrogées en
direct via des serveurs **MCP** (Model Context Protocol) :

- **Vesta** — le jumeau domotique de la maison (une quinzaine de sondes
  température/hygrométrie/CO₂, ouvrants, brasseurs de plafond, chaudière),
  qui lui permet aussi de déposer la recommandation du jour dans la maison ;
- **InfluxDB** — l'historique fin des capteurs, pour relire la nuit heure
  par heure (pentes de refroidissement, vérité des ouvertures, apports
  solaires mesurés) ;
- **Open-Meteo** — prévisions et qualité de l'air (ozone, particules) pour
  la station voisine de Meaux, modèles Météo-France.

Les instructions détaillées de l'orchestration (prompts, enchaînements,
outillage) ne sont volontairement pas publiées : ce dépôt contient les
rapports et leurs données, pas la recette.

## Repères méthodologiques

**Modèle thermique auto-calibrant.** La maison est traitée comme un circuit
RC : les murs (meulière, forte inertie) sont une batterie thermique de
capacité C, chargée le jour, déchargée la nuit. Chaque purge nocturne
observée fournit une constante de temps τ par pièce ; nuit après nuit, ces
mesures resserrent C, les capacités surfaciques par niveau et le débit réel
de ventilation traversante. Les énergies (kWh stockés/rendus) sont toujours
données en fourchettes, avec leurs hypothèses.

**Dose de chaleur et fenêtres d'ouverture.** L'intensité d'une journée se
mesure en degré-heures au-dessus d'un seuil (26 °C, 28 °C). Les créneaux
d'aération sont bornés par deux conditions simultanées : air extérieur plus
frais que la maison, et ozone sous le seuil OMS (100 µg/m³) — le rapport
publie ces fenêtres déjà digérées.

**Zone de confort cible.** La référence est le confort adaptatif
(EN 16798 / Givoni) : la cible suit la moyenne extérieure glissante des
7 derniers jours (0,31·T_moy + 17,8). Le ressenti est suivi en Humidex, et
les échanges d'air sont tracés par l'humidité absolue (g/kg), insensible aux
variations de température — c'est elle qui prouve qu'une pièce a réellement
été ventilée.

**Signaux discrets.** Avant toute conclusion, les anomalies sont croisées :
capteurs voisins, historique des recommandations, cohérence commande/état
des ouvrants, signatures d'occupation. Une valeur isolée est un bruit ; deux
capteurs concordants sont un signal.

## Données et vie privée

Les copies publiées sont anonymisées automatiquement (prénoms retirés,
coordonnées GPS supprimées, références d'outillage neutralisées). Les
mesures elles-mêmes — températures, hygrométries, CO₂, énergies — sont
authentiques et non retouchées.
