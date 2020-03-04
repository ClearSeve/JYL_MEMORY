# ini

import configparser

cfg = configparser.ConfigParser()
cfg.read("F:\\\\cfg.ini")
val = cfg.get("section","key")
cfg.add_section("section")
cfg.set("section","a","b")
cfg.write(open("F:\\\\cfg.ini","w"))