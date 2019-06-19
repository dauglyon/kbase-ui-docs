# Super Quick Start for Narrative

## [1] make project directory anywhere

```
mkdir -p ~/work/project
cd ~/work/project
```

## [2] Clone repos:

```
git clone -b docker-multi-stage https://github.com/kbase/kbase-ui
git clone https://github.com/eapearson/kbase-ui-proxy
git clone -b develop https://github.com/kbase/narrative
```

## [3] Build and run kbase-ui:

```
cd kbase-ui
make docker-image build=dev
make run-docker-image env=dev
```

> Note: keep the docker container running in this window; each app will get it's own window.

## [4] Build and run narrative:

open a new terminal tab or window (recent versions of Terminal or iTerm should open new windows within the same directory)

```
cd ../narrative
make dev_image
```

If successful, this will finish with an error, which as the following line states, you may ignore:

```
curl: (7) Failed to connect to localhost port 443: Connection refused
Ignore Error
```

> "Ignore Error" prints if the "curl" error printed above occurs; if there are errors above the "curl" line, they should **not** be ignored!

run the script

```
env=ci bash scripts/local-dev-run.sh
```

## [5] Build and run the proxy

open a new terminal tab or window

```
cd ../kbase-ui-proxy
make docker-image
local_narrative=true make run-docker-image env=dev
```

## [6] Local host settings

Edit your local /etc/hosts file to point ci to the running kbase-ui-proxy.

```
vi /etc/hosts
```

insert this line at the end if not already there.

```
127.0.0.1	ci.kbase.us
```

I find it handy to have this in my /etc/hosts and uncomment as needed:

```
127.0.0.1	ci.kbase.us
#127.0.0.1	appdev.kbase.us
#127.0.0.1	next.kbase.us
#127.0.0.1	kbase.us narrative.kbase.us
```

## [8] Run with it

You should now be able to pull up kbase-ui on `https://ci.kbase.us` and navigate to the Narrative.

Since the proxy is using a dynamically-generated self-signed certificate to provide https, you will find that the non-private mode for most browsers is troublesome. Less troublesome is private mode for Safari and Chrome; Firefox does not work well at all even in private mode.


## Notes

- build times are longer than purely host-based, due to the cold caches for npm, bower, and probably pip package installers.