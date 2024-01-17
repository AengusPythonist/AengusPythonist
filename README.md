- 👋 Hi, I’m @AengusPythonista
- 👀 I’m interested in python coding
- 🌱 I’m currently learning how to code Python on the Pythonista app
- 💞️ I’m looking to collaborate on SpriteNode creation
- 📫 How to reach me - npcnation9@gmail.com
- 😄 Pronouns: hell/naw - fuck/this
- ⚡ Fun fact: I touch grass

-:) Below is a code I created using Pythonista (note the app is made ONLY for ios systems like ipad and iphone). By the way Im broke so i dont have a keyboard😩
  <!---
AengusPythonist/AengusPythonist is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#AengusPythonista
#Russian Roulette game :)
import console
import time
import random
import math
import sound
from scene import *
import ui
import canvas
A = Action


# Define global variables
player_turn = 1

class ButtonNode(SpriteNode): 
	def __init__(self, title, *args, **kwargs):
		SpriteNode.__init__(self, 'pzl:Button1', *args, **kwargs)
		button_font = ('Copperplate', 15)
		self.title_label = LabelNode(title, font=button_font, color='black', position=(0, 1), parent=self)
		self.title = title
		
		
		
class MenuScene(Scene):
	def __init__(self, title, subtitle, button_titles):
		Scene.__init__(self)
		self.title = title
		self.subtitle = subtitle
		self.button_titles = button_titles
		self.level = 1
	def setup(self):
		button_font = ('Copperplate', 15)
		title_font = ('Copperplate', 25)
		num_buttons = len(self.button_titles)
		self.x = 500
		self.y = 500
		self.bg = SpriteNode(color='blue', parent=self)		#line that contols the background.
		bg_shape = ui.Path.rounded_rect(0, 0, 240, num_buttons * 64 + 140, 8)
		bg_shape.line_width = 4
		shadow = ((0.45, 0.33, 1.0), 0, 0, 24)
		self.menu_bg = ShapeNode(bg_shape, (1.0, 1.0, 1.0), (0.0, 0.0, 0.0), shadow=shadow, parent=self)
		self.y1 = 100
		self.x1 = 500
		
		self.title_label = LabelNode(self.title, font=title_font, color='black', position=(0, self.menu_bg.size.h / 2 - 40), parent=self.menu_bg)
		self.title_label.anchor_point = (0.5, 1)
		
		self.subtitle_label = LabelNode(self.subtitle, font=button_font, position=(0, self.menu_bg.size.h / 2 - 100), color=(0.0, 0.0, 0.0), parent=self.menu_bg)
		self.subtitle_label.anchor_point = (0.5, 1)
		
		self.buttons = []
		for i, title in enumerate(reversed(self.button_titles)):
			btn = ButtonNode(title, parent=self.menu_bg)
			btn.position = 0, i * 64 - (num_buttons - 1) * 32 - 50
			self.buttons.append(btn)
			
		self.did_change_size()
		self.menu_bg.scale = 0
		self.bg.alpha = 0
		self.bg.run_action(A.fade_to(0.4))
		self.menu_bg.run_action(A.scale_to(1, 0.3, TIMING_EASE_OUT_2))
		self.bullets = random.randrange(6)
		self.background = 'green'
		
	def did_change_size(self):
		self.bg.size = self.size + (2, 2)
		self.bg.position = self.size / 2
		self.menu_bg.position = self.size / 2
		
	def touch_began(self, touch):
		touch_loc = self.menu_bg.point_from_scene(touch.location)
		for btn in self.buttons:
			if touch_loc in btn.frame:
				btn.texture = Texture('spc:ButtonRed')
				
				if btn.title == 'Shoot':
					self.shoot_bullet()
				elif btn.title == 'Spin':
					self.spin_chamber()
				else:
					sound.play_effect('rpg:MetalClick')
										
	def touch_ended(self, touch):
		sound.stop_all_effects()
		touch_loc = self.menu_bg.point_from_scene(touch.location)
		for btn in self.buttons:
			btn.texture = Texture('pzl:Button1')
			if self.presenting_scene and touch_loc in btn.frame:
				new_title = self.presenting_scene.menu_button_selected(btn.title)
				if new_title:
					btn.title = new_title
					btn.title_label.text = new_title
					
	def end_game(self):
		if self.level == 1:
			background('green')
			text("GAME OVER",'Zapfino', 34, 550,700)
					
								
								
	def shoot_bullet(self):
		if self.bullets:
			bullet_chance = random.randint(1, 6)
			if bullet_chance == 6:
				# Bullet fires
				console.clear()
				sound.play_effect('barrett_m107_sniper_rifle-AmMoe-432217390.mp3')
				print("GAME OVER")
			
				
			else:
				# "Bullet" does not fire
				console.clear()
				sound.play_effect('Dry-Pistol-Gunfire-Single-A-www.fesliyanstudios.com.mp3')
				print("No shot fired. Player turn switch")
				
	def spin_chamber(self):
		sound.play_effect('clean-revolver-reload-6889.mp3', 1, 1, 1, True)
		# Reset the list of "bullets"
		self.bullets = ['Bullet'] * 6
		random.shuffle(self.bullets)
			
			

RussianRoulette = MenuScene('Russian Roulette', 'Life or death is in your hands', ['Spin', 'Stop', 'Shoot'])
run(RussianRoulette, show_fps=True)
