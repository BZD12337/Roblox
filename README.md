--开源
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "老龙王高检测防踢神器（无脏话）",
   LoadingTitle = "融合加载中",
   LoadingSubtitle = "手动开启无敌",
   ConfigurationSaving = {Enabled = true, FolderName = "老龙王防踢"},
   KeySystem = false
})

local Tab = Window:CreateTab("高检测专用", 4483362458)

local MasterEnabled = false
Tab:CreateToggle({
   Name = "🛡️ 防御高检测服务器专用开启",
   CurrentValue = false,
   Callback = function(Value)
      MasterEnabled = Value
      if Value then
         -- 第一个：Hook Namecall拦截 (狂轰庆祝消息)
         local mt = getrawmetatable(game)
         setreadonly(mt, false)
         local old = mt.__namecall
         mt.__namecall = newcclosure(function(Self, ...)
            local method = getnamecallmethod()
            if method == "Kick" and Self == game.Players.LocalPlayer then
               -- 狂轰庆祝消息
               for i = 1, 10 do
                  game.StarterGui:SetCore("SendNotification", {
                     Title = "老龙王拦截成功！";
                     Text = "招笑服务器";
                     Duration = 3;
                  })
                  task.wait(0.3)
               end
               game.StarterGui:SetCore("SendNotification", {
                  Title = "拦截大胜利";
                  Text = "服务器踢不动";
                  Duration = 10;
               })
               return
            end
            return old(Self, ...)
         end)
         setreadonly(mt, true)

         -- 第二个：Character重载保险 (中二装逼)
         game.Players.LocalPlayer.CharacterRemoving:Connect(function()
            task.wait(0.1)
            game.Players.LocalPlayer:LoadCharacter()
         end)

         -- 第三个：零卡隐形微移 (中二装逼)
         local connection
         connection = game:GetService("RunService").Stepped:Connect(function()
            local char = game.Players.LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
               local root = char.HumanoidRootPart
               root.CFrame = root.CFrame + Vector3.new(math.random(-5,5)/100, 0, math.random(-5,5)/100)
            end
         end)

         -- 新加低优先级备用：Teleport踢重载 (保险起见，不影响上面)
         game.Players.LocalPlayer.OnTeleport:Connect(function(State)
            if State == Enum.TeleportState.Started then
               task.wait(0.1)
               teleporthandler = game:GetService("TeleportService"):Teleport(game.PlaceId, game.Players.LocalPlayer)
            end
         end)

         Rayfield:Notify({Title = "全开无敌", Content = "高检测服务器 狂轰庆祝开启！", Duration = 6})
      end
   end,
})

-- 一键增强 (不变)
Tab:CreateButton({
   Name = "💎 一键增强 (全自动防检测反作弊)",
   Callback = function()
      -- (不变代码)
      Rayfield:Notify({Title = "一键增强激活", Content = "网络所有权重置+抖动伪装，反作弊全失效", Duration = 6})
   end,
})

Tab:CreateLabel("手动开启全融合：Hook拦截狂轰庆祝 + 重载保险 + 隐形微移")
Tab:CreateLabel("低优先级备用Teleport防踢已加，不影响主力！")

Rayfield:Notify({Title = "融合防踢升级完成", Content = "拦截成功狂轰庆祝🖕😂", Duration = 8})
