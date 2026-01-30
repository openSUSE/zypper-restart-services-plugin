#!/usr/bin/python3
"""
SPDX-FileCopyrightText: 2026 Ralf Habacker
SPDX-License-Identifier: GPL-2.0-or-later

ZYpp commit plugin: run /usr/libexec/zypper/restart-services after package updates
"""

import subprocess
import sys
from zypp_plugin import Plugin
import logging
from pathlib import Path

LOGFILE = Path("/var/log/zypp/restart-services-plugin.log")

logger = logging.getLogger("zypper-restart-services-plugin")
logger.setLevel(logging.INFO)
file_handler = logging.FileHandler(LOGFILE)
file_handler.setFormatter(logging.Formatter("%(asctime)s - %(levelname)s - %(message)s"))
logger.addHandler(file_handler)

def log(msg):
    logger.info(msg)

def run_restart_services():
    try:
        log("[restart-services] starting")
        process = subprocess.Popen(
            ["/usr/libexec/zypper/restart-services"],
            stdout=subprocess.PIPE,
            stderr=subprocess.STDOUT,
            universal_newlines=True
        )
        for line in process.stdout:
            log(line.rstrip())
        retcode = process.wait()
        if retcode != 0:
            log(f"[restart-services] exited with code {retcode}")
            return False
        log("[restart-services] finished")
        return True
    except Exception as e:
        log(f"[restart-services] failed: {e}")
        return False

class PostUpdatePlugin(Plugin):
    def PLUGINBEGIN(self, headers, body):
        log("Plugin initialization (PLUGINBEGIN)")
        self.ack()

    def COMMITBEGIN(self, headers, body):
        log("Commit starting (COMMITBEGIN)")
        self.ack()

    def COMMITEND(self, headers, body):
        pkgs = []
        if isinstance(body, list):
            for pkg in body:
                name = pkg.get("name") or pkg.get("package") or "<unknown>"
                ver  = pkg.get("version") or "<unknown>"
                pkgs.append(f"{name}-{ver}")
        if pkgs:
            log(f"Commit finished, packages updated: {', '.join(pkgs)}")
        else:
            log("Commit finished, no packages updated")
        self.ack()

    def PLUGINEND(self, headers, body):
        log("Plugin ending (PLUGINEND), running post-update actions")
        success = run_restart_services()
        if not success:
            self.error({}, "restart-services failed")
        self.ack()

if __name__ == "__main__":
    plugin = PostUpdatePlugin()
    plugin.main()
