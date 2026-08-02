ARG BUILD_FROM=ghcr.io/home-assistant/aarch64-base:latest
FROM $BUILD_FROM

# Java 21 installieren (ARM64)
RUN apk add --no-cache openjdk21-jre curl bash

WORKDIR /mc

# Fabric Installer herunterladen und Server installieren
RUN FABRIC_INSTALLER=$(curl -s https://meta.fabricmc.net/v2/versions/installer | \
      python3 -c "import sys,json; data=json.load(sys.stdin); print(data[0]['url'])") && \
    curl -L "$FABRIC_INSTALLER" -o fabric-installer.jar && \
    java -jar fabric-installer.jar server -mcversion 1.20.1 -downloadMinecraft && \
    rm fabric-installer.jar

# EULA akzeptieren
RUN echo "eula=true" > eula.txt

# Mods-Ordner erstellen
RUN mkdir -p mods

# Geyser + Floodgate (Fabric)
RUN curl -L "https://download.geysermc.org/v2/projects/geyser/versions/latest/builds/latest/downloads/fabric" \
      -o mods/Geyser-Fabric.jar && \
    curl -L "https://download.geysermc.org/v2/projects/floodgate/versions/latest/builds/latest/downloads/fabric" \
      -o mods/Floodgate-Fabric.jar

# ViaFabric (server-seitige Via-Unterstützung)
RUN VIAFABRIC_URL=$(curl -s "https://api.modrinth.com/v2/project/viafabric/version?game_versions=[\"1.20.1\"]&loaders=[\"fabric\"]" | \
      python3 -c "import sys,json; v=json.load(sys.stdin); print(v[0]['files'][0]['url'])") && \
    curl -L "$VIAFABRIC_URL" -o mods/ViaFabric.jar

# Create Fabric + Abhängigkeiten
RUN CREATE_URL=$(curl -s "https://api.modrinth.com/v2/project/create-fabric/version?game_versions=[\"1.20.1\"]&loaders=[\"fabric\"]" | \
      python3 -c "import sys,json; v=json.load(sys.stdin); print(v[0]['files'][0]['url'])") && \
    curl -L "$CREATE_URL" -o mods/Create-Fabric.jar

# Fabric API (Pflicht für die meisten Mods)
RUN FABRICAPI_URL=$(curl -s "https://api.modrinth.com/v2/project/fabric-api/version?game_versions=[\"1.20.1\"]&loaders=[\"fabric\"]" | \
      python3 -c "import sys,json; v=json.load(sys.stdin); print(v[0]['files'][0]['url'])") && \
    curl -L "$FABRICAPI_URL" -o mods/FabricAPI.jar

# Startskript
COPY run.sh /mc/run.sh
RUN chmod +x /mc/run.sh

EXPOSE 25565/tcp
EXPOSE 19132/udp

CMD ["/mc/run.sh"]
