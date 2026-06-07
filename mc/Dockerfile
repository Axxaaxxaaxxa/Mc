FROM itzg/minecraft-server:latest

ENV TYPE=FABRIC \
    VERSION=1.21.1 \
    EULA=TRUE \
    FABRIC_LOADER_VERSION=0.16.5 \
    MODRINTH_PROJECTS="fabric-api,create-fabric,viafabric,viabackwards,viarewind,geyser,floodgate" \
    INIT_MEMORY=512M \
    MAX_MEMORY=2G \
    JVM_XX_OPTS="-XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:G1NewSizePercent=20 -XX:G1ReservePercent=20 -XX:G1HeapRegionSize=32M"

EXPOSE 25565/tcp
EXPOSE 19132/udp
