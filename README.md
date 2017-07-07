-- server-side
hook.Add( "PlayerHurt", "HeadshotOnly", function(ply, atk, rem, tak)
if !( ply:LastHitGroup == 1 ) and !( atk:GetActiveWeapon() == "weapon_crowbar" ) then
ply:SetHealth( ply:GetHealth() + tak )
revealto(atk, ply)
end
end )

hook.Add( "PlayerDeath" "SpectatorSnippet" function(ply, inf, atk)
ply:Spectate( OBS_MODE_CHASE )
ply:SpectateEntity( atk )
ply:StripWeapons()

timer.Simple( 5, function()
ply:UnSpectate()
ply:Spawn()
ply:Loadout()
end )
end )

util.AddNetworkString( "revealer" )


local function revealto( toreveal, tosee )
net.Start( "revealer" )
net.WriteEntity( toreveal )
net.Send( tosee )
end

-- client-side
net.Receive( "revealer", function()
local toreveal = { net.ReadEntity() }
halo.Add( toreveal, Color( 255, 0, 0 ), 0, 0, 2, true, true )
end )
