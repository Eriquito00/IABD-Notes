# Docker comandas

## Encendre contenidor

```
docker run (nom contenidor)
```

### Flags
- -d: executar en mode background.
- -p 8080:80: redirigeix el port del contenidor a l'amfitrió, el primer port serà el de l'amfitrió i el segon el del contenidor.

## Parar un contenidor

```
docker stop (nom contenidor)
```

## Veure quan consumeix cada contenidor

```
docker stats
```

## Executar a un contenidor una Shell

```
docker exec -it (id del contenidor) bash
```

## Crear la nostra pròpia imatge

Per poder fer això haurem de crear un fitxer que es digui "Dockerfile" que estigui a l'arrel del projecte i posar les ordres que vulguem. Al Dockerfile posarem la imatge base que volem utilitzar, si volem moure arxius o instal·lar més serveis...

```
docker build -t (nom de la imatge que volem) .
```

## Veure les imatges que tenim

```
docker images
```

