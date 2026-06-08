GameStore Mobile
Arthur Amorim e Maria Luisa

import React, { useState } from 'react';
import { View, Text, TextInput, TouchableOpacity, StyleSheet, Alert } from 'react-native';

export default function Estatisticas() {
  const [likes, setLikes] = useState(0);
  const [commentsCount, setCommentsCount] = useState(0);
  const [commentText, setCommentText] = useState('');
  const [isFavorite, setIsFavorite] = useState(false);

  const handleLike = () => {
    setLikes(likes + 1);
  };

  const handleDislike = () => {
    setLikes(prevLikes => (prevLikes > 0 ? prevLikes - 1 : 0));
  };

  const handleSendComment = () => {
    if (commentText.trim() === '') {
      Alert.alert('Atenção', 'Por favor, digite um comentário antes de enviar.');
      return;
    }

    setCommentsCount(commentsCount + 1);
    setCommentText(''); // Limpa o campo de texto
    Alert.alert('Sucesso', 'Comentário enviado com sucesso!');
  };

  return (
    <View style={styles.container}>
      {/* Botões de Interação */}
      <View style={styles.row}>
        <TouchableOpacity style={styles.button} onPress={handleLike}>
          <Text style={styles.buttonText}>👍 Curtir ({likes})</Text>
        </TouchableOpacity>

        <TouchableOpacity style={styles.button} onPress={handleDislike}>
          <Text style={styles.buttonText}>👎 Descurtir</Text>
        </TouchableOpacity>

        <TouchableOpacity 
          style={[styles.button, isFavorite && styles.buttonFavorite]} 
          onPress={() => setIsFavorite(!isFavorite)}
        >
          <Text style={styles.buttonText}>
            {isFavorite ? '⭐ Favorito' : '☆ Favoritar'}
          </Text>
        </TouchableOpacity>
      </View>

      <Text style={styles.commentCount}>💬 {commentsCount} comentário(s)</Text>
      
      <View style={styles.commentBox}>
        <TextInput
          style={styles.input}
          placeholder="Escreva um comentário..."
          placeholderTextColor="#888"
          value={commentText}
          onChangeText={setCommentText}
        />
        <TouchableOpacity style={styles.sendButton} onPress={handleSendComment}>
          <Text style={styles.sendButtonText}>Enviar</Text>
        </TouchableOpacity>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    marginTop: 15,
    paddingTop: 10,
    borderTopWidth: 1,
    borderTopColor: '#333',
  },
  row: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 12,
  },
  button: {
    backgroundColor: '#252529',
    paddingVertical: 8,
    paddingHorizontal: 12,
    borderRadius: 6,
    flex: 1,
    marginHorizontal: 4,
    alignItems: 'center',
  },
  buttonFavorite: {
    backgroundColor: '#4d3b00',
  },
  buttonText: {
    color: '#fff',
    fontSize: 13,
    fontWeight: 'bold',
  },
  commentCount: {
    color: '#aaa',
    fontSize: 13,
    marginBottom: 6,
  },
  commentBox: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  input: {
    backgroundColor: '#252529',
    color: '#fff',
    flex: 1,
    height: 40,
    borderRadius: 6,
    paddingHorizontal: 10,
    fontSize: 14,
  },
  sendButton: {
    backgroundColor: '#007ACC',
    height: 40,
    justifyContent: 'center',
    paddingHorizontal: 15,
    borderRadius: 6,
    marginLeft: 8,
  },
  sendButtonText: {
    color: '#fff',
    fontWeight: 'bold',
    fontSize: 14,
  },
});


import React, { useState } from 'react';
import { View, Text, Image, TouchableOpacity, StyleSheet } from 'react-native';
import Estatisticas from './Estatisticas';

export default function Jogo({ nome, categoria, plataforma, imagem, data, descricao }) {
  const [showDescription, setShowDescription] = useState(false);

  return (
    <View style={styles.card}>
      <Image source={{ uri: imagem }} style={styles.image} />
      
      <View style={styles.content}>
        <Text style={styles.title}>{nome}</Text>
        
        <View style={styles.badgeRow}>
          <Text style={styles.badge}>{categoria}</Text>
          <Text style={styles.badgePlatform}>{plataforma}</Text>
        </View>

        <Text style={styles.releaseDate}>Lançamento: {data}</Text>

        <TouchableOpacity 
          style={styles.toggleButton} 
          onPress={() => setShowDescription(!showDescription)}
        >
          <Text style={styles.toggleButtonText}>
            {showDescription ? '▲ Ocultar Descrição' : '▼ Mostrar Descrição'}
          </Text>
        </TouchableOpacity>

        {showDescription && (
          <Text style={styles.description}>{descricao}</Text>
        )}

        <Estatisticas />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  card: {
    backgroundColor: '#1c1c1e',
    borderRadius: 12,
    marginBottom: 20,
    overflow: 'hidden',
    borderWidth: 1,
    borderColor: '#2c2c2e',
  },
  image: {
    width: '100%',
    height: 180,
    backgroundColor: '#2c2c2e',
  },
  content: {
    padding: 16,
  },
  title: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#fff',
    marginBottom: 8,
  },
  badgeRow: {
    flexDirection: 'row',
    marginBottom: 8,
  },
  badge: {
    backgroundColor: '#007ACC',
    color: '#fff',
    paddingHorizontal: 8,
    paddingVertical: 4,
    borderRadius: 4,
    fontSize: 12,
    fontWeight: 'bold',
    marginRight: 6,
  },
  badgePlatform: {
    backgroundColor: '#34c759',
    color: '#fff',
    paddingHorizontal: 8,
    paddingVertical: 4,
    borderRadius: 4,
    fontSize: 12,
    fontWeight: 'bold',
  },
  releaseDate: {
    color: '#8e8e93',
    fontSize: 12,
    marginBottom: 12,
  },
  toggleButton: {
    paddingVertical: 6,
    alignSelf: 'flex-start',
  },
  toggleButtonText: {
    color: '#0a84ff',
    fontWeight: '600',
    fontSize: 14,
  },
  description: {
    color: '#d1d1d6',
    fontSize: 14,
    lineHeight: 20,
    marginTop: 8,
    textAlign: 'justify',
  },
});

import React from 'react';
import { StyleSheet, Text, View, FlatList, SafeAreaView, StatusBar } from 'react-native';
import Jogo from './componentes/Jogo';

const JOGOS_DATA = [
  { id: '1', nome: 'Minecraft', categoria: 'Sandbox', plataforma: 'PC', descricao: 'Jogo de construção em mundo aberto baseado em blocos.', imagem: 'https://picsum.photos/id/1/300/180', data: '01/06/2026' },

  { id: '2', nome: 'Grand Theft Auto V', categoria: 'Ação/Aventura', plataforma: 'PS5', descricao: 'Explore o mundo aberto de Los Santos e execute grandes assaltos.', imagem: 'https://picsum.photos/id/10/300/180', data: '15/05/2026' },

  { id: '3', nome: 'The Witcher 3', categoria: 'RPG', plataforma: 'Xbox Series X', descricao: 'Acompanhe Geralt de Rívia em sua busca pela criança da profecia.', imagem: 'https://picsum.photos/id/20/300/180', data: '10/04/2026' },

  { id: '4', nome: 'Elden Ring', categoria: 'Soulslike', plataforma: 'PC', descricao: 'Viaje pelas Terras Intermédias para se tornar o próximo Lorde Prístino.', imagem: 'https://picsum.photos/id/30/300/180', data: '22/03/2026' },

  { id: '5', nome: 'Cyberpunk 2077', categoria: 'RPG/Ação', plataforma: 'PC', descricao: 'Uma história de ação e aventura de mundo aberto em Night City.', imagem: 'https://picsum.photos/id/40/300/180', data: '18/02/2026' },

  { id: '6', nome: 'Stardew Valley', categoria: 'Simulação', plataforma: 'Nintendo Switch', descricao: 'Crie a fazenda dos seus sonhos e interaja com os moradores locais.', imagem: 'https://picsum.photos/id/50/300/180', data: '05/01/2026' },

  { id: '7', nome: 'Red Dead Redemption 2', categoria: 'Ação/Aventura', plataforma: 'PS4', descricao: 'Uma história épica sobre a vida no implacável coração dos EUA.', imagem: 'https://picsum.photos/id/60/300/180', data: '12/12/2025' },

  { id: '8', nome: 'God of War Ragnarök', categoria: 'Ação', plataforma: 'PS5', descricao: 'Kratos e Atreus devem viajar pelos Nove Reinos em busca de respostas.', imagem: 'https://picsum.photos/id/70/300/180', data: '09/11/2025' },

  { id: '9', nome: 'Hollow Knight', categoria: 'Metroidvania', plataforma: 'PC', descricao: 'Forje seu próprio caminho em um reino em ruínas de insetos e heróis.', imagem: 'https://picsum.photos/id/80/300/180', data: '25/10/2025' },

  { id: '10', nome: 'Valorant', categoria: 'FPS', plataforma: 'PC', descricao: 'Um jogo de tiro tático focado em personagens e habilidades.', imagem: 'https://picsum.photos/id/90/300/180', data: '14/09/2025' },

  { id: '11', nome: 'Hades', categoria: 'Roguelike', plataforma: 'Xbox One', descricao: 'Desafie o deus dos mortos enquanto batalha para sair do Submundo.', imagem: 'https://picsum.photos/id/100/300/180', data: '30/08/2025' },

  { id: '12', nome: 'Among Us', categoria: 'Casual', plataforma: 'Mobile', descricao: 'Descubra quem é o impostor a bordo da nave espacial com seus amigos.', imagem: 'https://picsum.photos/id/110/300/180', data: '19/07/2025' },

  { id: '13', nome: 'Celeste', categoria: 'Plataforma', plataforma: 'Nintendo Switch', descricao: 'Ajude Madeline a enfrentar seus demônios internos na jornada ao topo da montanha.', imagem: 'https://picsum.photos/id/120/300/180', data: '02/06/2025' },

  { id: '14', nome: 'Terraria', categoria: 'Sandbox', plataforma: 'PC', descricao: 'Cave, lute, explore e construa neste universo repleto de aventuras.', imagem: 'https://picsum.photos/id/130/300/180', data: '15/05/2025' },

  { id: '15', nome: 'Resident Evil 4 Remake', categoria: 'Survival Horror', plataforma: 'PS5', descricao: 'Leon S. Kennedy é enviado para resgatar a filha raptada do presidente.', imagem: 'https://picsum.photos/id/140/300/180', data: '20/04/2025' },

  { id: '16', nome: 'Spider-Man 2', categoria: 'Ação', plataforma: 'PS5', descricao: 'Balance por Nova York jogando como Peter Parker e Miles Morales.', imagem: 'https://picsum.photos/id/150/300/180', data: '10/03/2025' },

  { id: '17', nome: 'Forza Horizon 5', categoria: 'Corrida', plataforma: 'Xbox Series X', descricao: 'Explore as paisagens vibrantes e em constante evolução do México.', imagem: 'https://picsum.photos/id/160/300/180', data: '28/02/2025' },
  { id: '18', nome: 'League of Legends', categoria: 'MOBA', plataforma: 'PC', descricao: 'Batalhe em equipes estratégicas no mapa de Summoner\'s Rift.', imagem: 'https://picsum.photos/id/170/300/180', data: '14/01/2025' },

  { id: '19', nome: 'Horizon Forbidden West', categoria: 'Ação/RPG', plataforma: 'PS5', descricao: 'Junte-se a Aloy para desbravar uma fronteira majestosa, porém perigosa.', imagem: 'https://picsum.photos/id/180/300/180', data: '05/12/2024' },

  { id: '20', nome: 'FIFA 24 / EA FC 24', categoria: 'Esporte', plataforma: 'PS5', descricao: 'A mais realista experiência de simulação de futebol do mercado moderno.', imagem: 'https://picsum.photos/id/190/300/180', data: '25/11/2024' }
];

export default function App() {
  return (
    <SafeAreaView style={styles.container}>
      <StatusBar barStyle="light-content" />
      
      <View style={styles.header}>
        <Text style={styles.headerTitle}>GameStore Mobile</Text>
        <Text style={styles.headerSubtitle}>Catálogo Oficial de Jogos</Text>
      </View>

      <FlatList
        data={JOGOS_DATA}
        keyExtractor={item => item.id}
        renderItem={({ item }) => (
          <Jogo
            nome={item.nome}
            categoria={item.categoria}
            plataforma={item.plataforma}
            imagem={item.imagem}
            data={item.data}
            descricao={item.descricao}
          />
        )}
        contentContainerStyle={styles.listContent}
        showsVerticalScrollIndicator={false}
      />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#000',
  },
  header: {
    padding: 20,
    backgroundColor: '#1c1c1e',
    borderBottomWidth: 1,
    borderBottomColor: '#2c2c2e',
    alignItems: 'center',
  },
  headerTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#007ACC',
  },
  headerSubtitle: {
    fontSize: 14,
    color: '#8e8e93',
    marginTop: 4,
  },
  listContent: {
    padding: 16,
  },
});
