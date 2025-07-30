import React, { useState } from 'react';
import { View, Text, TextInput, Button, FlatList, StyleSheet, Alert } from 'react-native';

export default function App() {
  const [saldo, setSaldo] = useState(1000000);
  const [historial, setHistorial] = useState([]);
  const [destinatario, setDestinatario] = useState('');
  const [monto, setMonto] = useState('');

  const enviarDinero = () => {
    const cantidad = parseFloat(monto);
    if (!destinatario || isNaN(cantidad) || cantidad <= 0) {
      Alert.alert('Error', 'Por favor, ingresa monto válido');
      return;
    }
    if (cantidad > saldo) {
      Alert.alert('Error', 'Saldo insuficiente');
      return;
    }
    setSaldo(saldo - cantidad);
    setHistorial([
      { id: Date.now().toString(), destinatario, monto: cantidad.toFixed(2) },
      ...historial,
    ]);
    Alert.alert('Éxito', `Enviado $${cantidad.toFixed(2)} a ${destinatario}`);
    setDestinatario('');
    setMonto('');
  };

  return (
    <View style={styles.container}>
      <Text style={styles.titulo}>Monedero Virtual</Text>
      <Text style={styles.saldo}>Saldo disponible: ${saldo.toFixed(2)}</Text>

      <TextInput
        style={styles.input}
        placeholder="Destinatario"
        value={destinatario}
        onChangeText={setDestinatario}
      />
      <TextInput
        style={styles.input}
        placeholder="Monto a enviar"
        value={monto}
        onChangeText={setMonto}
        keyboardType="numeric"
      />
      <Button title="Enviar Dinero" onPress={enviarDinero} />

      <Text style={styles.historialTitulo}>Historial de Transferencias</Text>
      {historial.length === 0 ? (
        <Text>No hay transferencias</Text>
      ) : (
        <FlatList
          data={historial}
          keyExtractor={item => item.id}
          renderItem={({ item }) => (
            <View style={styles.itemHistorial}>
              <Text>{item.destinatario}: ${item.monto}</Text>
            </View>
          )}
        />
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20, paddingTop: 40 },
  titulo: { fontSize: 24, fontWeight: 'bold', marginBottom: 20, textAlign: 'center' },
  saldo: { fontSize: 18, marginBottom: 20, textAlign: 'center' },
  input: {
    borderWidth: 1,
    borderColor: '#69359C',
    borderRadius: 6,
    padding: 10,
    marginBottom: 12,
  },
  historialTitulo: { fontSize: 20, marginTop: 30, marginBottom: 10 },
  itemHistorial: {
    backgroundColor: '#C9A0DC',
    padding: 10,
    marginBottom: 8,
    borderRadius: 5,
  },
});
