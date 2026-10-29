# Diferencies de MongoDB amb SQL

MongoDB emmagatzema en format BSON que és bàsicament el format JSON però en binary. Però això limita que els seus documents no poden ser més grans de 16 MB.

MongoDB no garanteix:
- L'ordre dels camps
- És sensible als tipus de dades
- És sensible a majúscules ("edat", "Edat", "EDAT" són camps diferents)

Podem tenir una base de dades amb Mongo de tres formes:
- MongoDB Atlas: plataforma cloud de Mongo per poder fer projectes no massa exigents.
- Community Edition: versió gratuïta on-premise, instal·lable amb Docker.
- Enterprise Advanced: versió de pagament amb, suport, miniaturització...

