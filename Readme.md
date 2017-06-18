# キーボードの設定

xkbswitchをインストールする
[https://github.com/myshov/xkbswitch-macosx](https://github.com/myshov/xkbswitch-macosx)


Hammerspoonをインストールして設定
```
local function keyStroke(mods, key)
  return function() hs.eventtap.keyStroke(mods, key, 0) end
end

local function enterESCWithDisablingIME()
  hs.execute('xkbswitch -se ABC', true)
  hs.eventtap.keyStroke({"ctrl"}, "[")
end

local function remap(mods, key, fn)
  return hs.hotkey.bind(mods, key, fn, nil, fn)
end

-- disable when a specific application is active
local remapKeys = {
  remap({}, 'ESCAPE', enterESCWithDisablingIME)
}

local function handleGlobalAppEvent(name, event, app)
  if event == hs.application.watcher.activated then
    if name == 'Xcode' or name == 'Atom' then
      for i, key in ipairs(remapKeys) do
        key:enable()
      end
    else
      for i, key in ipairs(remapKeys) do
        key:disable()
      end
    end
  end
end

appsWatcher = hs.application.watcher.new(handleGlobalAppEvent)
appsWatcher:start()
```
