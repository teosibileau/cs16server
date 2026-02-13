FROM steamcmd/steamcmd:ubuntu

LABEL maintainer "Teofilo Sibileau <teo.sibileau@gmail.com>"

ARG steam_user=anonymous
ARG steam_password=
ARG metamod_version=1.20
ARG amxmod_version=1.8.2

# 📦 Installing curl for downloading mods...
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# 🎮 Installing HLDS with exponential backoff (6 retries, 5s→300s)
RUN mkdir -p /opt/hlds && \
    DELAY=5 && \
    for i in 1 2 3 4 5 6; do \
        echo "🎮 Attempt $i/6: Installing HLDS..." && \
        steamcmd +login $steam_user $steam_password \
            +force_install_dir /opt/hlds \
            +app_update 90 validate \
            +quit && \
        echo "✅ HLDS installed successfully!" && \
        exit 0; \
        echo "❌ Attempt $i failed, waiting ${DELAY}s..." && \
        sleep $DELAY && \
        DELAY=$((DELAY * 2 > 300 ? 300 : DELAY * 2)); \
    done; \
    echo "💥 ERROR: HLDS installation failed after 6 attempts!" && \
    exit 1

# 🔍 Verifying HLDS installation
RUN [ -f /opt/hlds/hlds_run ] || \
    (echo "💥 CRITICAL: hlds_run binary not found!" && exit 1) && \
    echo "✅ HLDS verified: hlds_run binary present"

# 📄 Adding configuration files
ADD files/steam_appid.txt /opt/hlds/steam_appid.txt
ADD hlds_run.sh /bin/hlds_run.sh
RUN chmod +x /bin/hlds_run.sh && \
    echo "✅ Entrypoint configured!"

# 🗺️ Adding maps
ADD maps/* /opt/hlds/valve/maps/

# 🔌 Installing MetaMod
RUN mkdir -p /opt/hlds/valve/addons/metamod/dlls && \
    curl -sqL "http://prdownloads.sourceforge.net/metamod/metamod-$metamod_version-linux.tar.gz?download" | tar -C /opt/hlds/valve/addons/metamod/dlls -zxvf - && \
    echo "✅ MetaMod installed!"
ADD files/liblist.gam /opt/hlds/valve/liblist.gam
ADD files/plugins.ini /opt/hlds/valve/addons/metamod/plugins.ini

# 🛡️ Installing dproto
RUN mkdir -p /opt/hlds/calce/addons/dproto && \
    echo "✅ dproto directories created!"
ADD files/dproto_i386.so /opt/hlds/valve/addons/dproto/dproto_i386.so
ADD files/dproto.cfg /opt/hlds/valve/dproto.cfg

# 🔧 Installing AMX Mod X
RUN curl -sqL "http://www.amxmodx.org/release/amxmodx-$amxmod_version-base-linux.tar.gz" | tar -C /opt/hlds/valve/ -zxvf - && \
    echo "✅ AMX Mod X installed!"
ADD files/maps.ini /opt/hlds/valve/addons/amxmodx/configs/maps.ini

# 🧹 Cleaning up temporary files
RUN rm -rf /tmp/* /var/tmp/*

WORKDIR /opt/hlds

# 🚀 Entrypoint configured!
ENTRYPOINT ["/bin/hlds_run.sh"]
