# Local Docker Registry

Used to cache the images that are frequently used, so they do not have to be pulled from the internet.
Specially useful for devs that work in places with slow connection or want to save data from their phones.

## Configuration

### Docker for Mac

#### 1. Configure Docker for Mac

1. Open the Docker Preferences
2. Go to Docker Engine
3. Add this to the configuration config file

```json
{
  "insecure-registries": ["http://localhost:5555"],
  "registry-mirrors": ["http://localhost:5555"]
}
```

The port is defined in the docker-compose file.

#### 4. Run the Mirror

Run:
`docker compose up`

or
`docker compose up -d`

If you want the mirror in the background

#### 5. Hydrate the mirror

[ Requires internet connection ]

Start pulling the images you need.
If they are already downloaded they will not be pulled from the registry and therefore they will not be stored in the mirror so you want to remove all images once.

After that, start pulling images (or execute docker-compose up that will also pull the images)

As a simple example run:

`docker pull ubuntu:20.04`

#### 6. Verify that the mirror is working

The folder `.docker` should contain some images. For example, if we pull `ubuntu:20.04` we should have some folders and files under:
`.docker/registry/docker/registry/v2/repositories/library/ubuntu`

#### 7. Test the mirror

Delete all images again (or the ones you want to test) with the typical docker command. For example:

`docker image remove ubuntu:20.04`

Go offline and run again:

`docker pull ubuntu:20.04`

The image should be installed in your system via the mirror.

#### 8. Extra test

Check that being online and with the mirror stopped you can still pull images.

### About

This was tested with Docker Engine for Mac v20.10.17
