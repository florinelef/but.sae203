rapport rédigé par Malori ALVAREZ, Florine Lefebvre et Nathanaël COLLUMEAU

pour compiler en HTML
- télécharger pandoc
- compiler en HTML en utilisant la commande:
pandoc --standalone --template src/template-compilation-s203.html src/rapport-s203-markdown.md -o rapport-s203.html

pour compiler en PDF
- télécharger pandoc et pdflatex
- compiler en latex en utilisant la commande:
pandoc -N src/rapport-s203-markdown.md -o src/rapport-s203-latex.tex
- rajouter les lignes suivantes en tête du fichier .tex obtenu:
\documentclass{report}
\usepackage[utf8x]{inputenc}
\begin{document}
- rajouter la ligne suivante à la fin du même fichier .tex:
\end{document}
- compiler en PDF avec la commande:
pdflatex src/rapport-s203-latex.tex 