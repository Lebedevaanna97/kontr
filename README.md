# kontr
import tkinter as tk from tkinter
import ttk import webbrowser
import siteparser
class MainApp(tk.Tk):    
def __init__(self, *args, **kwargs):       
tk.Tk.__init__(self, *args, **kwargs)   
self.title('Отдохни')   
self.tabs = {}     
self.main_container = tk.Frame(self, borderwidth=1, relief="sunken", pady=10)    
self.main_container.pack(fill=tk.X, side=tk.TOP)
self.scan_btn = tk.Button(self.main_container, text='Сканировать "otdoxni.com"', padx=10) 
self.scan_btn.bind("&lt;Button-1>", self.scan_btn_click) 
self.scan_btn.pack(expand=1, fill="both") 
self.notebook = ttk.Notebook(self.main_container)    
self.notebook.bind("&lt;Button-1>", self.tab_changed) 
self.notebook.pack(expand=1, fill="both") 

# Init methods:     
def init_tab(self, frame, sp_obj):   
container = tk.Frame(frame, borderwidth=1, relief="sunken", pady=10) 
container.pack(fill=tk.X, side=tk.TOP) 
addr_text = 'Адрес:\n{}'.format(sp_obj.address)
tk.Label(container, text=addr_text).grid(column=0, row=0, stick='w')
time_text = 'Часы работы:\n{}'.format(siteparser.SportObjectFactory.work_time)  
tk.Label(container, text=time_text).grid(column=1, row=0)
sections_text = 'Секции:'   
tk.Label(container, text=sections_text).grid(row=1, columnspan=2)  
for i, sec in enumerate(sp_obj.sections): 
tk.Label(container, text=sec.name).grid(row=i+2, column=0, stick='w')        
lnk_btn = tk.Button(container, text='Перейти на страницу', padx=10) 
lnk_btn.bind("&lt;Button-1>", self.goto_sec(sec.href))   
lnk_btn.grid(row=i+2, column=1, stick='w')    

# Event handlers:   
def scan_btn_click(self, ev):   
self.scan_btn.unbind("&lt;Button-1>")       
self.scan_btn.config(state=tk.DISABLED)        
self.update_idletasks()         
siteparser.SportObjectFactory.scan_site()    
for name, obj in siteparser.SportObjectFactory.sport_objects_dict.items():      
tab = ttk.Frame(self.notebook)         
self.notebook.add(tab, text=name)           
self.tabs[obj] = tab        
obj.parse_info()         
self.init_tab(tab, obj)  
self.update_idletasks() 

def tab_changed(self, ev):     
# print(dir(ev))   
# tk.update()      
# print(dir(tk))    
print( "MyTab: click at (" + str(ev.x) + ", " + str(ev.y) + ') for ' + 'self.name' + " (parent name: " + self.notebook.tab(tk.CURRENT)['text'] + ")")    

def goto_sec(self, href):      
def goto_href(ev):           
webbrowser.open(href)           
# print(href)       
return goto_href 

if __name__ == "__main__":  
app = MainApp()    
app.mainloop()
