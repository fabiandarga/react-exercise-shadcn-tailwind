# Übung: Erstellung eines Nachrichtenformulars mit shadcn/ui und Tailwind CSS

## Einführung

In dieser Übung wirst Du Dein bestehendes "NewsPortal"-Projekt um eine neue Funktion erweitern. Du sollst eine neue Seite erstellen, die es Nutzern ermöglicht, neue Nachrichtenartikel hinzuzufügen. Dafür wirst Du die Komponenten-Bibliothek shadcn/ui verwenden und diese mit Tailwind CSS nach Deinen Vorstellungen stylen.

## Lernziele

- Kennenlernen und Implementieren von shadcn/ui Komponenten
- Anpassen von vordefinierten Komponenten mit Tailwind CSS
- Erstellung eines funktionsfähigen Formulars mit verschiedenen Eingabefeldern
- Integration in ein bestehendes Next.js Projekt

## Teilaufgabe 1: Projektstruktur vorbereiten

1. Navigiere zu Deinem "NewsPortal"-Projekt
2. Erstelle einen neuen Ordner und eine neue Datei: `/app/news/add/page.jsx` (oder `.tsx` wenn Du TypeScript verwendest)
3. Erstelle die grundlegende Struktur der Seite mit einem Container und einem Titel:

```jsx
export default function AddNewsPage() {
  return (
    <div className="container mx-auto py-10">
      <h1 className="text-2xl font-bold mb-6">Neue Nachricht erstellen</h1>
      
      {/* Hier kommt Dein Formular */}
      
    </div>
  );
}
```

## Teilaufgabe 2: shadcn/ui einrichten

Shadcn/ui ist keine typische npm-Bibliothek, sondern ein Sammelsurium von Komponenten, die Du direkt in Dein Projekt kopieren kannst. Dadurch hast Du volle Kontrolle über den Code und kannst ihn nach Belieben anpassen.

1. Falls Du shadcn/ui noch nicht eingerichtet hast, installiere es mit:

```bash
npx shadcn@latest init
```

2. Bei der Einrichtung wähle Folgendes:
   - Als Stil wähle "Default"
   - Als Farbschema wähle eines, das zu Deinem NewsPortal passt (z.B. "slate")
   - Base color kann auf "Neutral" bleiben
   - Bestätige die Verwendung von CSS-Variablen und die Erstellung einer globals.css-Datei
   - Wähle als React-Server-Komponenten "yes" (bei Next.js Projekten)
   - Als Komponenten-Verzeichnis bestätige den Vorschlag "@/components"

## Teilaufgabe 3: Formularkomponenten installieren

Für Dein Formular benötigst Du verschiedene Komponenten. Installiere sie mit folgenden Befehlen:

```bash
npx shadcn-ui@latest add form
npx shadcn-ui@latest add input
npx shadcn-ui@latest add textarea
npx shadcn-ui@latest add select
npx shadcn-ui@latest add button
```

Diese Befehle fügen die entsprechenden Komponenten zu Deinem Projekt hinzu, und Du findest sie im Verzeichnis `components/ui`.

## Teilaufgabe 4: Formular-Schema mit React Hook Form erstellen

Shadcn/ui verwendet React Hook Form und Zod für die Formularvalidierung. Hier ist, wie Du das Schema und die Formularlogik einrichten kannst:

```jsx
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import * as z from "zod";

// 1. Definiere das Validierungsschema mit Zod
const formSchema = z.object({
  title: z.string().min(5, {
    message: "Der Titel muss mindestens 5 Zeichen lang sein.",
  }),
  text: z.string().min(20, {
    message: "Der Inhalt muss mindestens 20 Zeichen lang sein.",
  }),
  category: z.string({
    required_error: "Bitte wähle eine Kategorie aus.",
  }),
});

export default function AddNewsPage() {
  // 2. Initialisiere das Formular mit dem Schema
  const form = useForm({
    resolver: zodResolver(formSchema),
    defaultValues: {
      title: "",
      text: "",
      category: "",
    },
  });

  // 3. Definiere die Funktion zum Absenden des Formulars
  function onSubmit(values) {
    // Hier kannst Du die Werte an Deine API senden oder anderweitig verarbeiten
    console.log(values);
    // z.B.: router.push("/news"); // Zurück zur News-Übersicht
  }

  return (
    <div className="container mx-auto py-10">
      <h1 className="text-2xl font-bold mb-6">Neue Nachricht erstellen</h1>
      
      {/* Hier kommt Dein Formular */}
      
    </div>
  );
}
```

## Teilaufgabe 5: Formular-UI mit shadcn/ui erstellen

Jetzt verwendest Du die installierten Komponenten, um das Formular zu erstellen:

```jsx
import { Button } from "@/components/ui/button";
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";

// ... (bisheriger Code für formSchema, useForm, onSubmit)

return (
  <div className="container mx-auto py-10">
    <h1 className="text-2xl font-bold mb-6">Neue Nachricht erstellen</h1>
    
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-8">
        
        {/* Titel-Eingabefeld */}
        <FormField
          control={form.control}
          name="title"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Titel</FormLabel>
              <FormControl>
                <Input placeholder="Gib einen aussagekräftigen Titel ein..." {...field} />
              </FormControl>
              <FormDescription>
                Der Titel Deiner Nachricht sollte kurz und prägnant sein.
              </FormDescription>
              <FormMessage />
            </FormItem>
          )}
        />
        
        {/* Text-Eingabefeld */}
        <FormField
          control={form.control}
          name="text"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Text</FormLabel>
              <FormControl>
                <Textarea 
                  placeholder="Schreibe den Inhalt Deiner Nachricht..." 
                  className="min-h-32" // Hier wird die minimale Höhe mit Tailwind festgelegt
                  {...field} 
                />
              </FormControl>
              <FormDescription>
                Beschreibe Deine Nachricht ausführlich.
              </FormDescription>
              <FormMessage />
            </FormItem>
          )}
        />
        
        {/* Kategorie-Auswahl */}
        <FormField
          control={form.control}
          name="category"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Kategorie</FormLabel>
              <Select onValueChange={field.onChange} defaultValue={field.value}>
                <FormControl>
                  <SelectTrigger>
                    <SelectValue placeholder="Wähle eine Kategorie" />
                  </SelectTrigger>
                </FormControl>
                <SelectContent>
                  <SelectItem value="politik">Politik</SelectItem>
                  <SelectItem value="wirtschaft">Wirtschaft</SelectItem>
                  <SelectItem value="technologie">Technologie</SelectItem>
                  <SelectItem value="sport">Sport</SelectItem>
                  <SelectItem value="kultur">Kultur</SelectItem>
                </SelectContent>
              </Select>
              <FormDescription>
                Ordne Deine Nachricht einer passenden Kategorie zu.
              </FormDescription>
              <FormMessage />
            </FormItem>
          )}
        />
        
        {/* Absende-Button */}
        <Button type="submit">Nachricht veröffentlichen</Button>
      </form>
    </Form>
  </div>
);
```

## Teilaufgabe 6: Styling mit Tailwind CSS

Jetzt kannst Du das Formular mit Tailwind CSS nach Deinen Wünschen gestalten. Hier sind einige Beispiele, wie Du das machen kannst:

1. Erstelle einen ansprechenden Card-Container für das Formular:

```jsx
<div className="container mx-auto py-10 max-w-3xl">
  <h1 className="text-3xl font-bold mb-8 text-center">Neue Nachricht erstellen</h1>
  
  <div className="bg-card text-card-foreground rounded-lg shadow-lg p-6 border border-border">
    <Form {...form}>
      {/* Formularinhalt wie zuvor */}
    </Form>
  </div>
</div>
```

2. Gestalte die Formularfelder neu:

```jsx
<FormField
  control={form.control}
  name="title"
  render={({ field }) => (
    <FormItem>
      <FormLabel className="text-lg font-medium">Titel</FormLabel>
      <FormControl>
        <Input 
          placeholder="Gib einen aussagekräftigen Titel ein..." 
          className="text-base focus:ring-2 focus:ring-primary/50 transition-all" 
          {...field} 
        />
      </FormControl>
      <FormDescription className="text-sm text-muted-foreground italic">
        Der Titel Deiner Nachricht sollte kurz und prägnant sein.
      </FormDescription>
      <FormMessage className="font-medium text-destructive" />
    </FormItem>
  )}
/>
```

3. Passe den Button an:

```jsx
<Button 
  type="submit" 
  className="w-full md:w-auto py-2.5 text-base font-semibold hover:bg-primary/90 transition-colors"
>
  Nachricht veröffentlichen
</Button>
```

4. Füge ein responsives Layout hinzu:

```jsx
<div className="grid grid-cols-1 md:grid-cols-2 gap-6">
  {/* Titel-Feld */}
  <div className="md:col-span-2">
    <FormField
      control={form.control}
      name="title"
      render={({ field }) => (
        {/* ... */}
      )}
    />
  </div>
  
  {/* Text-Feld */}
  <div className="md:col-span-2">
    <FormField
      control={form.control}
      name="text"
      render={({ field }) => (
        {/* ... */}
      )}
    />
  </div>
  
  {/* Kategorie-Auswahl */}
  <div className="md:col-span-1">
    <FormField
      control={form.control}
      name="category"
      render={({ field }) => (
        {/* ... */}
      )}
    />
  </div>
  
  {/* Button-Container */}
  <div className="md:col-span-2 flex justify-end mt-4">
    <Button type="submit">Nachricht veröffentlichen</Button>
  </div>
</div>
```

## Teilaufgabe 7: Zusätzliche Funktionen und Verbesserungen

Du kannst Dein Formular mit weiteren Funktionen erweitern:

1. Füge einen "Abbrechen"-Button hinzu:

```jsx
<div className="flex justify-end gap-4 mt-6">
  <Button variant="outline" type="button" onClick={() => router.push("/news")}>
    Abbrechen
  </Button>
  <Button type="submit">Veröffentlichen</Button>
</div>
```

2. Füge einen Fortschrittsindikator hinzu:

```jsx
// Am Anfang der Komponente
const [isSubmitting, setIsSubmitting] = useState(false);

// In der onSubmit Funktion
async function onSubmit(values) {
  setIsSubmitting(true);
  try {
    // API-Aufruf oder andere Logik
    await new Promise(resolve => setTimeout(resolve, 1000)); // Simuliere API-Aufruf
    console.log(values);
    // Erfolg
  } catch (error) {
    console.error(error);
  } finally {
    setIsSubmitting(false);
  }
}

// Im Button
<Button type="submit" disabled={isSubmitting}>
  {isSubmitting ? (
    <>
      <svg className="animate-spin -ml-1 mr-2 h-4 w-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
        <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
        <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
      </svg>
      Wird veröffentlicht...
    </>
  ) : (
    "Veröffentlichen"
  )}
</Button>
```

## Teilaufgabe 8: Integration und Testen

1. Überprüfe, ob alle Imports korrekt sind
2. Teste das Formular mit verschiedenen Eingaben
3. Überprüfe die Validierung und Fehlermeldungen
4. Stelle sicher, dass das Styling auf verschiedenen Bildschirmgrößen gut aussieht


## Hilfreiche Ressourcen

- [Shadcn/ui Dokumentation](https://ui.shadcn.com/)
- [Tailwind CSS Dokumentation](https://tailwindcss.com/docs)
- [React Hook Form Dokumentation](https://react-hook-form.com/)
- [Zod Dokumentation](https://zod.dev/)
